---
title: "On Toil"
date: 2022-05-09T19:01:24+01:00
draft: true
tags: [ "SRE", "toil", "automation" ]
---

# TOIL

- What is toil: work you don't want to do.
  - Code review: merge gates are a good example of automating toil.

- < 50% of your time on toil
  - this is an errata from the book. should actually be < 50% on all ops work,
  of which toil is one bucket.

- Feature dev work
  - some SREs are feature devs, depends on the team.
    - There are internal software systems which are SRE owned.
    - There are also user facing products with dedicated dev teams where SREs
      regularly contribute feature code.

- SRE vs DevOps
  - SREs are "ops focused". Nope. SREs are production focused.

- Calculating toil
  - oncall and interrupts are not necessarily toil. they're operations, but
    they may not be toil.
  - note also: if you have oncall/tickets shifts that are quiet, you should be
  using that time to reduce operational burden.
    - have a "sharp edges" or "small fixes" list.
      - fixing alerts, improving dashboards
      - improving tooling (release/rollout workflows, diagnostic tools, etc)
    - things that are explicitly not in scope:
      - Working on your current project work (unless your project fits into
        toil reduction)

- Investigating novel errors is not toil, because it's not (yet) repeatable.
  - the second or third or fiftieth time it's toil.

- Outlaw: QA shouldn't be doing manual stuff.

- Why is toil bad?
  - Career development. Hard to sell promotion on "I turned the crank."
  - Too much toil: hurts the SRE team.
    - Confusion: the team is annoyed, but other groups get the wrong impression.
