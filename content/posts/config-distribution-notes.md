---
title: "Config Distribution Notes"
date: 2026-09-09T11:53:11+01:00
draft: true
tags: [ "config distribution", "distributed systems", "sre" ]
---

I've had several long conversations about the properties of config distribution systems recently. Rather than regurgitate them from scratch every time, let me dump the skeleton of the conversation into a post I can link people to in future.


# Topics

There are a bunch of interlinked concerns here. Here's an index of topics and one sentence descriptions.

- API - what is a "config"? How are they named/addressed?
- Publication - how do config producers ship their configs and mark them "live"
- Distribution - how does the infrastructure ship configuration around, what kind of persistence do you want
- Discovery/subscription - how do config consumers find the current version of the config they're supposed to load (or conversely, how does the infrastructure discover consumers it needs to push to)
- Rollout policies and safety checks - what stuff should be in the config system vs what should be on tenant configurations to validate and guarantee
  - This assumes a multi-tenant config system. Maybe you're not building that yet.
- Config distribution is not necessarily an experiments framework, and not all configurations have to do with traffic.
  - You might use your config distribution system to ship experiment configuration around to consumers, but actually configuring experiments is its own thing.
- Bootstrap/startup - what do consumer processes do when they start up?
  - What do they do when they start up and there's a rollout of a config they care about currently in progress?
- Client side persistence/caching - how many old configs do clients keep around?
- Rollback actutation/policy - how do rollbacks work, how quickly can they be actuated?

# API

You probably end up with a key/value model, with keys mapping to configs. Values are (probably) going to be opaque to the configuration system, other than perhaps having a very loose set of constraints that allows the infrastructure to detect fundamental corruption. (e.g. all configs must be valid UTF8 strings/JSON/text protobufs/whatever)

Sometimes you end up with a directory hiearchy, either a real one or just a fake one based on prefix matching key names. This is useful for things like permissions which you want to be able to delegate, or set at some higher point and then have them be inherited down the tree, but it's not necessary.

# Publication

Shipping configs and marking them live. You want to think about the dependencies you want to incur for config push. e.g. lots of places offer config/featureflags/experiments as a SaaS service. This seems bonkers to me vs just shipping configs to your consumers directly and not depending on another SaaS product and public internet connectivity between your dev machines/the config SaaS/the cloud(s) your production workloads run in. But I have on-prem/not-invented-here syndrome. So don't just trust my opinion.

# Distribution
I have seen two models:
- client library in your serving process
- a sidecar config subscribtion process

The tradeoffs here are typically config size and persistence in the face of control plane outage.

# Client library in your serving process
This is what it sounds like. You import a client library, give it key(s) you're interested in. That triggers a subscribiption to the config system (or registers with the infrastructure, depending on push vs pull)

With this approach, everything is typically in memory with no local persistence. By implication you don't have copies of historical configs - you have the current live config, and sometimes you have the one that's in the process of being distributed but isn't live yet.

# Sidecar
Meanwhile the sidecar model is typically for data that's a lot larger (MiB/GiB), where propagation/replication of a new config takes a long time, and you want to be able to handle network/process blips without restarting entirely from scratch. You also want to be able to validate new configuration asynchronously.

Your sidecar which persists stuff locally.  It's typically coscheduled with your serving thingo (same container or a sibling container, if you want resource isolation). It writes new config/data into a directory, and then the serving process either watches the filesystem for new writes, or waits for a SIGHUP to reload config.

One of the nice properties of this model is that you get local persistence "for free". This means that e.g. if you have a serving process crash and restart in the same pod on the same node, it can just read the last config from disk and start serving without waiting for a new push (assuming the config didn't cause the crash :P ). That does assume some node level affinity for tasks. If your serving process is going to be rescheduled onto a different node on every restart, then these benefits don't apply, obviously.

Another benefit is that rollbacks to configs in your retention window are super quick and cheap. Send an RPC to your sidecar, it flips a symlink for "current live config" to point to a previous revision still on disk, sends a SIGHUP to your serving binary, and you're away.

# Rollout policies/safety checks

If you're building a general purpose/multi-tenant config distribution system, it typically can't do any semantic validation of what's being shipped around. At best it can confirm syntactic validity if you give it some metadata (e.g. the key should be a valid protobuf, and here's the proto definition file for this config), but... your client can also do that at load time and reject invalid configs. (And your config generation pipeline should be doing that before publishing anything new to consumers).

What your config distribution should have is:

1. liveness checks. Make sure that your client process doesn't crash for e.g. 10 seconds after acknig the load of a new config
   - this is very easy with a streaming client library implementation, because you just check if the RPC peer goes away (assuming your RPC timeout is lower than your config liveness timeout)
   - with a sidecare it's a bit more annoying, your sidecar needs to be hitting a health endpoint on the serving process or something
2. healthy/serving/staleness checks. "I have a valid, live config from ${TIMESTAMP}, and I am [capable of] doing useful work"

On (2) there should be a contract with the consumer about how they indicate "new config is invalid" which the config distribution system can back propagate up. Optionally, you could have a "stage new config" RPC and a "set new config live" RPC - but often that's probably more complexity than it's worth.

By default you want to select an arbitrary canary population for each config push, push and set live the new configs to all canaries.
- If one or more canaries dies during the liveness check interval, immediately fail the push and roll back.
- If they all survive the liveness check interval and then return successful healthchecks indicating they've loaded the new config, then proceed with the rollout to the non-canary population.
  - If you have particularly large client sets for some configs such that you have a low level of process failures/reschedules all the time, then it may make sense to be a bit lax with canary failures. e.g. 1000 subscribers, 5 canaries, up to 2 canary failures is fine, or something.

## Duration, config skew

In terms of config skew, there are a few variables:
- do you allow multiple inflight pushes of the same config at the same time (my general take - no. if you need that, then you're in the realm of binary deploys or possibly feature flags, not dynamic config)
- how long can an individual push last?
- what do you do about new tasks that come up during a config rollout? etc

there are varying philosophies here.

One take is "set everything live everywhere all at once! short pushes FTW" - because skew in production is hard to code for and hard to reason about. The counter to this is "you'll always have some amount of skew, because physics, so your system has to deal with it. Also, there are low probability failures that take some time to present, and it's better to have them present before your rollout finishes". As an example, you could imagine a config push system that ships a new config on a regular 3 hour cadence, has a 10 minute canary phase for each push, and the post-canary push is smeared over a 30 minute window just in case something goes wrong that the canary didn't catch.

## Policy explosion

While most of these system properties should be configurable, you almost certainly don't want to allow customers/tenants to choose arbitrary combinations of values. You generally want two opinionated, generally useful defaults:

- low frequency/slow
- high frequency/fast

optionally with some policy about what types of config fit into each bucket.

If anyone wants to deviate from the menu, they need a really good reason, and their set of preferences should be added to the precanned menu.

# Bootstrap/startup

This ties in to how long your individual pushes are, and semantically when you mark a new config as live. One option is "after canaries have suceeded". Another is "after the full prod push is done". The latter can get dicey with large numbers of subscribers, because at thousands of consumers, some tasks will just go away/get autoscaled down.

Either way, you need to make sure you understand how you deal with a straggler - that is, a new process that comes up mid push which grabs the old/being replaced config. Does your system ensure that all such processes get the latest config before the next push starts? Or does that just run an old config until the next push?

# Topology

Mechanically, both in terms of distribution as well as discovery, you typically end up with your config system topology matching the underlying cluster/region/AZ/whatever infrastructure. That is, you will have an instance of the config store in every k8s cluster/AWS AZ etc.

Most of the time, clients talk to the config store in their local cluster. This is a natural way to control fan in, and it makes the typical discovery case trivial. (Have a k8s service discovery end point, or an ILB service you configure in every VPC, or ...)

It also gives you a natural distribution tree. 3 possible patterns:
- hub and spoke, with a "global" which serves as upstream/authoritative/origin. People write new configs to that, and those flow downstream to each cluster config service (push or pull, doesn't really matter)
- mesh - no "global", instead every cluster component task to every other one, and you have some consensus/conflict resolution protocol for how writes to the same value in different clusters are serialised.
- truly standalone. each cluster local thing only knows abot the files it contains, and its local clients. Your administrative control plane (e.g. a shell script on a dev laptop, or your rollout system like argocd) has to be able to talk to every cluster individually to actuate a global config push.

## Tradeoffs
The global hub and spoke is the simplest to implement and reason about. However it does give you a global component that fails globally. Also, bear in mind - even if you build it atop something like `etcd` or something else backed by a consensus protocol and spread across regions or cloud providers - anything that's taking part in a state replication protocol has to be considered a single software failure domain.

The "mesh in prod" vs "talks to every cluster" modles are just variants on an idea, which is "don't have a global config distribution component in prod". Of the two (if you're dead set against a global distribution hub in prod), mesh in prod is more work, because you really need to be on top of your replication semantics. That being said, I marginally prefer it to "config pushes from your rollout system", because to me, runtime dynamic config pushes like billing or ACLs or whatever are things that should happen continuously/autonomously. (vs a binary rollout which typically happens on a slower cadence, and may stop over weekends, holiday/freeze periods, etc).

## Lifecycle management

Related to both bootstrap and topology is lifecycle management: what does turnup/turndown look like?

With a hub and spoke model, it's pretty easy - adding a new cluster is just "tell global where the new thing is" (or tell each new cluster how to reach global). The full mesh case is annoying, because you have an N² config problem: every new cluster turnup means reconfiguring the config service in every existing cluster.

# Client side persistence/caching

TODO. It's handy, but then you have distributed persistent state (and consistency/staleness to worry about).

# Rollbacks
## Actuation

TODO. RPC and flip a symlink and HUP? RPC and replace in memory config?

## Policy

All rollbacks should be fast, safe and cheap. This goes quadruple for config rollbacks. You should just rollback every config at random once a day or so to prove that it remains so.
