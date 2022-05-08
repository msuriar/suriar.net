+++
title = "OpenJDK Hackday"
date = 2014-01-20T00:14:00Z
updated = 2014-01-20T00:14:58Z
tags = ["learning", "coding", "openjdk"]
+++

Since I am currently possessed of an excess of free time, I’m always looking
for events to participate in which will be entertaining and/or educational.
Yesterday, this led me to attend a
[hackday](http://en.wikipedia.org/wiki/Hackathon) run by the [London Java
Community](http://www.meetup.com/Londonjavacommunity/).

These hackdays are based around
[OpenJDK](http://en.wikipedia.org/wiki/OpenJDK), the [reference
implementation](https://blogs.oracle.com/henrik/entry/moving_to_openjdk_as_the)
for Java Standard Edition. People generally work on whatever they like (it is a
hackday, after all), but one of the main goals of these regular hackdays is to
provide an environment where neophytes to OpenJDK development can try and start
making contributions. There are a handful of active contributors to the OpenJDK
codebase around to provide support and guidance to people getting familiar with
the language.

For myself, the last time I wrote Java code was in my final year of university,
which was a Very Long Time Ago. I also have no burning desire to pick up Java
development again. This leads to an obvious question: what exactly am I doing
spending time at a hackday for the reference implementation of Java?

The short version: it’s all [Richard’s](http://insightfullogic.com/blog/)
fault.

The slightly longer version: Richard had mentioned the OpenJDK hackday at the
pub a few weeks ago, and tried to dragoon several of us into coming along. When
I objected that I wasn’t a Java developer (arguably I’m not any kind of
developer, but that’s a larger problem), he promptly shut me down:

> It’s fine! We need help sorting out build tools and stuff. Chef! Vagrant!
> Devops! You know about that kind of low level rubbish!

(I may be taking some liberties with the details, but I maintain that this
captures the core of his argument.)

And so it was with some trepidation that I made the long trip to Shoreditch on
Saturday morning; not entirely sure what to expect, and somewhat doubtful that
I would be of any use. I needn’t have worried. Everyone at the event was very
welcoming, and it turns out that one of the main problems that needs solving
doesn’t require any Java knowledge at all. (Namely: the problem of providing a
reproducible working build environment so that new contributors can quickly get
stuck in to OpenJDK develpment.)

We very quickly formed an “infrastructure” room, where between 5 and 10 of us
over the course of the day worked to try and make progress on producing a
deterministic [base virtual
machine](https://github.com/adoptopenjdk/openjdk-virtual-images) and a [build
environment](https://github.com/adoptopenjdk/openjdk-chef-build) to go on top
of it. I learned a great deal, and found myself actually able to make some
meaningful contributions.

There’s still lots of work to be done, and I suspect I’ll be working on making
a better OpenJDK development environment for some time to come.
