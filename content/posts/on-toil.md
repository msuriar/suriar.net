---
title: "On Toil"
date: 2022-05-13T14:01:24+01:00
tags: [ "SRE", "toil", "automation", "Google" ]
---

_(Well - this is a bit of a random way to get back to blogging. See
endnotes[^endnotes] for more details.)_

This is some commentary/exposition in response to [CodingBlocks] episode [184],
which is a podcast review of the [Eliminating Toil] chapter of the [Site
Reliability Engineering] book.

[CodingBlocks]: https://www.codingblocks.net/
[184]: https://www.codingblocks.net/episode184
[Eliminating Toil]: https://sre.google/sre-book/eliminating-toil/
[Site Reliability Engineering]: https://sre.google/books/

This isn't going to be an end to end review - you should listen to the podcast
and/or read the book chapter for that. Rather I'm going to call out and clarify
specific points based on my understanding and experience.

Obligatory: these opinions on my own, and do not necessarily reflect the views
of any of my employers past or present.

# What is toil?

As pointed out in the podcast, toil is not just _"work I don't want to do"_.
There are many thoughts and opinions here, but to address a specific comment
that [Allen] made (I'm paraphrasing):

[Allen]: https://twitter.com/theallenu

> Code review isn't toil.

I prefer the framing that _"code review **shouldn't** be toil"_.

Imagine mandatory code review processes which don't add any value - for
example, having a human check the generated output of a template change, in
addition to the template change itself. The correctness of the generated output
is something that can (and should) be validated automatically, using tests of
the template generator, for example. Similarly, style/lint/test coverage
can be caught by automated code analysis. Human code review time is scarce, and
so it should be spent on things of the highest value, and shouldn't be wasted
on things that computers can do for you.

Improving your code workflow tooling is absolutely a form of toil reduction.

# Toil budgets[sic]

The podcast also hit on a common misconception: that Google SRE mandates that
no more than 50% of each SRE's time is spent on toil. This is not quite
correct. Quoting from the book:

> Our SRE organization has an advertised goal of keeping __operational work__
> (i.e., toil) below 50% of each SRE’s time.

(emphasis mine).

tl;dr - not all operational work is toil. I actually think the _"(i.e., toil)"_
in the text is misleading, and is what often leads to this misconception.
Operational work includes everything about running a service which is not
engineering/project work. To list a few:

- oncall shifts
- tickets/interrupts shifts
- capacity planning for regular growth
- turning up new instances
- ...

This is all operational work. Not __all__ of it is toil though, because not all
of it meets the criteria of toil.

To provide a concrete example: imagine a fire in a datacenter which is going to
leave that building off line for 6+ months.  That's going to need incident
response from all the tenant services to spin up new capacity and come up with
a story for any single homed data (or even multi-homed data which needs
re-replication). Incident response in the face of an outage beyond the normal
operating design of a service is a thing you have to do. It's definitely
operational work, because it's not engineering/project work. However it's not
toil as per the criteria from the book:

- it's not repetitive
- it's not (entirely) automatable, certainly not at the macro level
- it doesn't grow at `O(n)` with service growth


However, toil often gets focused on when people try and achieve this "&lt;50%
of time on ops work" guideline, because toil is the sort of stuff that focused
engineering work can make go away.

# On what SREs do

Paraphrasing the podcast again: "SREs don't do feature work, they work on
reliability". Ehhhh... not quite.

Firstly, some features can add reliability, so it's not always an either/or.

On SREs at Google, and what if any feature work they do: this very much depends
on the team. It's very common for SREs at Google to work on improving
operational tooling (release/rollout systems, monitoring, etc), but there are
also cases where they work on the core products:

- Some internal systems are fully SRE supported.
- I'm aware of a handful of externally facing systems which have at times been
    SRE supported, due to the development team being spun down for there not being
    enough work.
- There are many products where SREs are able to make feature contributions to
    the core product beyond just reliability improvements.

# SRE, Ops, and DevOps

The debate about SRE and DevOps has raged in specific corners of the internet
for a long time.  I'm not sure I have any more to add here, but since it came
up in the podcast, I'll say that I broadly agree with [Allen]: DevOps is a way
of working. SRE is, variously:

- A job role (a human who improves reliability through engineering)
- An organisational structure (having a reliability org distinct from a dev
    org, both with senior leaders accountable for features and reliability
    respectively. This is part of the overal SRE approach that Google
    pioneered)
- A set of practices, as described in the SRE books and elsewhere.

Responding to a specific quote from the podcast: "SREs are ops focused": this
misunderstands what SRE (the role and approach) is. SREs are __production__
focused, but SRE teams are not _operations teams_ (in the sense that an
operations org is one that grows linearly as your service grows).

# Tickets

Working on tickets may or may not be toil, it depends on the circumstances.

- If you have 50 tickets a shift for quota increases from users, that's toil,
    and an improvement opportunity.
- If you have 5 tickets which are _"debug this latency event"_, and each event
    is unique and has different underlying triggers, then that's not toil.
    - Although there may be some toil-y data gathering steps which are common
        which you could automate.

# Time tracking and project planning

Google's SRE books make many assumptions about how organisations and teams are
structured. One that's relevant here: in Google's SRE model, one thing that (in
theory) is supposed to be universally true is that scheduled oncall/interrupts
shifts are **explicitly not for project work.** You shouldn't be planning to
get any project work done on those days. (As a corollary, managers, project
managers, program managers and so on should be budgeting for oncall when doing
macro level planning.)

If you're on tickets/interrupts, you'll probably have a good amount of stuff to
do. If you have a quiet tickets shift (or you're primary oncall, and your
rotation doesn't get paged often), you should be working on small tasks which
improve the lives of oncallers/ticketeers.

As a concrete example: a common thing I do during incident response is file
small bugs for alerting/monitoring/diagnostic/rollout automation improvements
which would make responding to a similar incident easier in future. Those bugs
are typically 1h-4h of work. Stick them on a "sharp edges"/"tool sharpening"
list, and make it clear that spare oncall time is supposed to be spent on that.

# On why toil is bad

The podcast mentioned career development. To expand on this a bit more - it's
hard to sell a promotion case around the statement "I turned the crank on this
machine that runs on human blood to do repetitive low value tasks".

If an SRE team has too much toil, the team will have low morale due to not
being able to deliver engineering improvements and other project work.
Additionally, it gives the wrong impression to partner teams. SRE is not an
operations function which throws additional people at problems as they grow.

If a service requires lots of toil to keep running, that type of work is the
first thing which should be handed back to the associated development team when
an SRE team is in operational overload. This of course presupposes that the
organisational structure and senior leadership supports such actions; this is
often true at Google, but may not be true in every organisation, which can
lead to difficult choices for individual SREs and their managers when faced
with operational overload.

A final underlying assumption at Google is that SREs are free to leave a
team/service which is not great to work on. If that happens too quickly in
succession, a team can drop below the minimum viable staffing for oncall
rotations, and suddenly development teams are responsible for production again.

This is true at Google, because there are many different SRE teams, and your
current manager generally can't block a transfer if you've been in post and
performing well for at least a year. However there are places that this breaks
down:

- your organisation doesn't let people transfer
- you're in an office with only one or two SRE teams
- you're in a company with only 5 SREs
- ...

In those cases, there should be other structural incentives to limit toil,
otherwise SREs who are feeling overloaded are likely to look for a new job
outside the company.

[^endnotes]: For the last several years I've been hanging out on the CodingBlocks
  [Slack] - it's a great community, I thoroughly recommend it. Over time, as
  and when the podcast covers topics that I'm familiar with, I've dropped some
  commentary in the `#episode-discussion` channel. This obviously kicked up a
  notch when the CodingBlocks folks started talking about the SRE book. My most
  recent notes and errata were long form enough that I thought... this is
  probably enough to turn into a blog, maybe? Sooo.... here it is? Hopefully
  it's of interest.

[Slack]: https://www.codingblocks.net/slack
