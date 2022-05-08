+++
title = "FOSDEM 2014"
date = 2014-02-06T19:36:00Z
updated = 2014-02-07T00:55:13Z
tags = ["conference", "fosdem"]
+++


This past weekend, I and several others made our way to
[ULB](http://www.ulb.be/ulb/presentation/uk.html) for [FOSDEM
2014](https://fosdem.org/2014/). FOSDEM is an annual conference aimed at the
free and open source software communities. In addition to talks on a wide
variety of topics, it doubles as a convenient place and time for developers on
projects who are spread all over the world to meet up in person.

I attended a bunch of talks over the weekend; below is a brief list and some
impressions based on my recollections and notes. Almost all the talks were
[recorded](http://video.fosdem.org/2014/); once they’ve been published, I’ll
come back to this blog and link them all.

# Talks
## Reproducible Builds in Debian

Binary packages in Debian are currently the opposite of reproducible. Two
consecutive builds of the same source package on the same machine will produce
different binaries. Most differences come from timestamps (in archive formats,
documentation, etc) but there are some other sources. There is an effort on
going at the moment to modify the tooling used to create binary packages in
Debian to eliminate these sources of variation. An experiment using these
modified build tools resulted in more than 60% of packages being identical.
That’s insanely impressive. It seems very possible that we could get to a
situation where 100% of packages are verifiable. If we could then create an
audited build image from first principles (or ideally two or three), it would
become significantly harder for an attacker to sneak undocumented behaviour
into these packages. Definitely one to watch.

## Autoscaling Best Practices

A talk about autoscaling; what it is in general, the various types of
autoscaling, what it’s good at, what it’s not so good at, and some of the
implementations available in the world. A solid high level overview which gives
good advice. Worth watching if you’re new to the concept of autoscaling
infrastructure. (TODO: video).

## Network Function Virtualisation and Network Service Insertion and Chaining

This was a somewhat theoretical talk about how the world changes when you
virtualise network services, using the various components of OpenStack to
provide examples.

Briefly: a network service is some capability which is provided by the network
to one or more application/host/hypervisor.  The most trivial network service
is that of basic connectivity; the ability to send packets elsewhere, although
obviously this isn’t that interesting.

Consider instead something a little more complex; say a network firewall, or a
load balancer. Traditionally these capabilities/services have been provided by
big centralised pieces of Magic Vendor Hardware; having a pair of firewalls and
a pair of load balancers for an entire datacenter is a fairly common (anti?)
pattern. This leads to a bunch of complexity and scaling issues.

For example: if you need to firewall flows between two applications in your
cluster, you need to drag those flows all the way up to your firewalls and then
back down again, even if the applications are running on two VMs/containers
scheduled on the same hypervisor. This then has implications for how much
bandwidth you need to provision from your top of rack switches to your cluster
edge and back. Your firewall policy also grows alarmingly; it is essentially
the union of all permitted flows to/from every node in your cluster. Such a
policy is going to become difficult to maintain and reason about, and quite
possibly is going to blow out the capacity of whatever BigCorp firewall
appliance you’re using, meaning that you soon have to start scaling it out.

A key point this talk makes, which bears emphasising: you don’t _need_
SDN for virtualised network services, but the two do complement each other
nicely.

Now, let’s pretend that this is a world where network functions are
virtualised, and that I’m spinning up a new SSL web service.

I need a new public IP, because SSL; no problem, my SDN control plane starts
advertising the new IP via BGP to the outside world, and my SDN controller
installs a flow on my edge routers which sends any traffic to this new IP to
some newly provisioned software load balancer instance which is running inside
the cluster. It also updates the iptables rules on the hypervisor that the
loadbalancer is running on, permitting inbound port 443 traffic to the new
public IP.

In the mean time, my cluster control infrastructure has spun up a reverse proxy
VM as well as a webserver on which I can run the actual website. Right now,
because no-one knows about this service, the load balancer, the reverse proxy,
and the web server VMs are all running on the same hypervisor. However, 6
months from now, when people actually care about this service, scaling it out
is a simple case of spinning up more instances of whatever the bottleneck
component is, combined with possibly updating some flow rules so that you can
have more than one load balancer.

Compare this to a world where you need to submit tickets for new hardware, new
firewall rules, some load balancer changes, some reverse proxy changes… a new
order is coming, folks. Adapt or die.

## Dovecot’s way of scaling to millions of users

This was an interesting talk by Dovecot’s maintainer about the various
implementation optimisations that have been made in order to get over
performance limitations in Dovecot. There was some interesting
conceptual/philosophical stuff here (e.g. everything should be asynchronous),
as well as good discussion about the architecture of a scaled Dovecot
installation. (Leaf nodes, proxies, directors). I’m reasonably certain that I’m
going to have to get back to self-hosting my own mail in the next few years, as
Gmail becomes more and more of a consumer product, so this was a useful
refresher on how real email works.

## SPARK 2014: Hybrid Verification using Proofs and Tests

An interesting talk on how to extend compiler and testing infrastructure. A lot
of it went over my head, since I don’t spend enough time coding, but the main
thrust of this seemed to be that contracts (decorators/hints to the compiler
which express preconditions/postconditions/invariants) allow you to move many
runtime assertion/check failures to compile time. (Trivial example; bounds
checking on array access). In addition, contracts allow you to skip a vast
swathes of trivial unit tests; e.g. the contract for an “increment” function
provides sufficient information to allow the compiler to auto-generate test
cases; we shouldn’t need to write them ourselves.

SPARK appears to include a bunch of interesting language constructs which aid
in producing safer, less fragile code. It will be interesting to see if
contracts or something like them make their way into more widely used
languages.

## Foreman
### Not a talk, rather a project I found out about.

[Foreman](http://theforeman.org/) is an opensource project for managing the
lifecycle of physical and virtual servers, from provisioning to operating to
monitoring to configuring to decommissioning. It looks like a mature project,
with significant support for both Puppet and Chef, as well as a bunch of
physical and cloud compute providers. Definitely a project to keep an eye on.

## Who ate my battery?
### Why free and open systems are solving the problem of excessive energy consumption.

This was a very thought provoking talk about the seeming disconnect between the
hardware and software worlds when it comes to power optimisation. The hardware
world has long included “power consumption” as one of the variables in the
optimisation problems that are being solved. In the “old” days, when embedded
hardware ran custom, purpose written software, such hardware features were made
use of as a matter of course. Now, more and more embedded devices run general
purpose operating systems and applications written in high level languages
which do not expose developers to the realities of embedded development. This
has a direct impact on the power efficiency of such devices.

This talk discussed various approaches to change this; starting with the
profiling of software combined with measuring power consumption, in order to
gather power efficiency data for various combinations of hardware, compiler,
compiler version, compiler optimisations… and so on. Once this profiling data
has been gathered, it can then be used to feed a machine learning system which
then generates a model with which to simulate upper and lower bounds for power
consumption of various algorithms/implementations of those algorithms, given a
set of the above constraints. Think of these as akin to back of the napkin
calculations based on [latency numbers every progammer should
know](http://www.eecs.berkeley.edu/~rcs/research/interactive_latency.html).

The end goal is to extend general purpose compilers like GCC and CLANG to
provide `-o power` optimisation modes, to produce binaries that run
efficiently in power constrained environments. The project as a whole is called
[MAGEEC](http://mageec.org/), and it looks very promising.

## The FOSDEM Network

FOSDEM is a unique environment; thousands of geeks descend on the ULB campus
every year to talk about all manner of interesting topics. There are no fees,
and no guest registration. Running a wireless network in such an environment
might be described as challenging. Only a crazy person would make it harder by
[making the wireless network IPv6 only by
default](https://fosdem.org/2014/news/2014-02-01-pushing-ipv6/).

…

Running a v6 only network by default isn’t something that’s really been done at
scale before, as far as I’m aware. Networking events like
[IETF](http://www.ietf.org/meeting/77/network.html) and
[NANOG](https://www.nanog.org/meetings/nanog43/pdf/NANOG43_Wireless_IPV6.pdf)
have had an IPv6 only SSID for a while, but those events self-select for people
who know about IPv6. FOSDEM, in contrast, is an event aimed at the open source
community, most of whom don’t care about IPv6. Setting the default SSID to be
IPv6 only, with [transition](http://tools.ietf.org/html/rfc6146)
[mechanisms](http://tools.ietf.org/html/rfc6147) in place to facilitate access
to the legacy IPv4 internet was a bold move; the good news is that it seems to
have gone extremely well. (Modulo a
[bug](http://code.google.com/p/android/issues/detail?id=32630) which meant
every Android user had to switch to the fallback FOSDEM-dualstack SSID. Boo!).
Also, it broke anyone validating DNSSEC; which
[Faux](https://blog.goeswhere.com/), [tom](https://tom-fitzhenry.me.uk/blog/)
and I spent far too much time discussing.

In addition to talking about how this was done, there were some interesting
stats about the number of unique devices, how much traffic FOSDEM sees, etc.
All in all very interesting (but then again, I am a network geek).

## Preventing craziness: a deep dive into OpenStack testing automation

For those who aren’t aware, [OpenStack](http://www.openstack.org/) is an “open
source cloud operating system”; it’s a collection of subprojects which, when
taken together, allow you to spin up your own private cloud on top of computer,
networking and storage resources.

This was a really impressive talk. Openstack is 2 million source lines of code
spread across 95+ code repositories, with 400 contributors, all of whom can
commit changes. In order to keep control over the chaos that results, the
people responsible for OpenStack’s project infrastructure have put together an
impressive set of processes and controls which allows them to cut a release
every 6 months and to have a master branch which is always usable despite
dozens of merges a day. I have reams of notes but the TL;DR is:

- Nothing gets merged without being code reviewed by two other people.
- Nothing gets to code review without tests passing.
- If something is not tested automatically, it’s not tested.
- If something is not tested, it doesn’t work.

Though I have reams of notes from this talk, I’m not going to replicate them
here. Instead, I highly recommend watching the talk when it’s published; this
is how to do rapid software development at scale, and the OpenStack team have
either integrated or created a bunch of open source tools to make it work
really well. This project has always intrigued me; now that I’ve seen how they
run things, I’m even more convinced that they’re going to end up being the
default open source cloud management solution.

## ANSTE: Advanced Network Service Testing Environment
### Testing Network Services in Multimachine Scenarios

This was a lightning talk on a [framework](http://www.openstack.org/) for
testing network services using a bunch of virtual machines. I don’t have reams
of notes, but it did seem to be fairly straightforward to use, and provided a
simple way for developers to spin up and configure interconnected VMs for the
purposes of testing network applications.

## Adventures with CloudStack and OpenDaylight

A talk about getting support for OpenDaylight into the CloudStack project. Some
background:

[OpenDaylight](http://www.opendaylight.org/) is an open source software defined
networking (SDN) platform. What is SDN? It’s a topic for another blog post, but
the short version is: a network which is configured by software applications
written by your own developers, rather than by software provided by your
network hardware vendor.

[CloudStack](http://cloudstack.apache.org/) is similar to OpenStack; it’s an
open source cloud management platform.

This talk chronicled the experience of one of the CloudStack developers in
getting support for provisioning networking for VMs OpenDaylight into
OpenStack. It was quite interesting, but did serve to demonstrate that
OpenDaylight still has quite a ways to go before it’s production ready.

## Tunnels as a Connectivity and Segregation Solution for Virtualized Networks

This was an excellent talk, in my opinion. It’s targeted at developers who
don’t have much/any understanding of networking below the `sockets`
API.

It sought to explain the importance of tunnelling in network solutions for
VM-VM communication in a virtualised environment. It started with some
historical background about how Ethernet works, and why VLANs are the hammer of
choice for many network engineers when it comes to segregation. It then went on
to explain that in a world where networking is moving up into the hypervisor,
VLANs aren’t necessarily the best choice for this anymore.

Overall an excellent explanation of how networking works in today’s virtualised
datacenters; stuff that every developer should know. I’ve given variants of
this talk at employers; it’s good to see this stuff being discussed in the
wider open source community.

## No more IPv4

A talk by IPv6 evangelist [Eric Vyncke](http://www.vyncke.org/) about the
implication of IPv4 exhaustion; the nature of an internet with ever-increasing
amounts of NAT, and how applications can be extended with relatively little
work to support IPv6.  Specifically, why should open source developers care
about IPv6? Because sooner or later, some of their customers will only have
IPv6 connectivity, or horribly broken, multiply NATted IPv4 connectivity.

As an aside: I was pleasantly surprised at the focus that IPv6 received at
FOSDEM this year; both this and the earlier talk about the FOSDEM network were
in large rooms and pretty full. Hopefully this reflects a growing interest in
IPv6 from the wider open source community.

## Using RIPE Atlas API for measuring IPv6 Reachability

[RIPE-NCC](http://www.ripe.net/) is the organisation responsible for
distributing number resource (IPv4, IPv6 addresses, AS numbers) to the European
internet community.  In addition to this primary responsibility, they also
provide a bunch of data and tools to the internet community, to assist service
providers with monitoring and analysing the internet. One of those tools is
[ATLAS](https://atlas.ripe.net/), a distributed system managed by RIPE NCC used
for running large scale measurement across the internet.

ATLAS is made up of a bunch of probes which run a stripped down, hardened Linux
distribution which are installed in host networks around the world. These
probes are then used by RIPE NCC (and members who sign up to use the API) to
run tests about internet reachability from devices all over the world. The data
from such tests is then made freely available to the internet community. The
[source code](https://atlas.ripe.net/get-involved/source-code/) for the probes
is freely available.

This talk provided some detail about what RIPE NCC and ATLAS are, and how to
use ATLAS to get information about IPv6 reachability on the internet. It was
very interesting; ATLAS itself seems like a very cool project; I’ve requested a
probe for myself, and will probably sponsor a few more once I get one.

## NSA operation ORCHESTRA: Annual Status Report

I can’t do this one justice; once the video is published, you just have to go
and watch it. :)

# Outro

Wow. That’s the longest blog I’ve written in a while. I may expand on some of
the talks/topics raised in future blog posts; I’m definitely going to continue
attending FOSDEM. The fact that such an event can be set up for free and
attract 5000+ developers from all over the world continues to amaze me. See
everyone next year!

(To come: my write up of [Configuration Management
Camp](http://cfgmgmtcamp.eu/), which occurred down the road in
[Hogent](http://english.hogent.be/) immediately following FOSDEM).
