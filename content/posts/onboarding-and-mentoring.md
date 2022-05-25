---
title: "Onboarding and mentoring"
date: 2022-05-25T11:00:00+01:00
tags: [ "mentoring", "teaching", "onboarding", "career development" ]
---

Another week, another blog courtesy of the [CodingBlocks] [Slack].

[Steven Leadbeater] asked this set of questions:

> Has anyone ever taken a step back from code to run a team of juniors? Is it
> possible to maintain your output by proxy in the early days of full time
> junior coaching? Can anyone who's done this offer any advice, pitfalls to
> look out for or upsides I might not be thinking of?

Unsurprisingly I have some opinions. Here they are, distilled a bit, and
combined with some good advice from Niklas Asp. Thanks to [John Looney] for
feedback on my first draft.

[Steven Leadbeater]: https://www.linkedin.com/in/steve-leadbeater/
[John Looney]: https://twitter.com/john_p_looney

# On productivity

> Is it possible to maintain your output by proxy ...

tl;dr - "no".

And that's fine. Time spent on onboarding/coaching/mentoring is an investment.
You're investing time now for significantly increased productivity, deferred by
some number of weeks or months.

One model I learned - I think from another colleague at Google? it's been a
while - is to consider your own output as a simple product of two things:

```
output = productivity * hours worked
```

Simple enough.

## Individual productivity

When you just think about your own personal output, then you can
get more efficient, or you can spend more time working.

Many people early in their career work a lot of hours - that may be because
they can, and they enjoy the work. It may also be because they feel they have
to (this is a cultural problem, but addressing that is a whole set of posts by
itself). In any case, there is still an upper limit to the number of hours one
person can work (let's call it 1.5x more hours than a "normal" work week).
That's before accounting for lifestyle changes which will reduce your
willingness/ability to work long hours.

There are things you can do to improve your own productivity - getting more
familiar with tools, languages, libraries. Getting more practice at design and
implementation. And so on.

But eventually, you'll reach a limit of improved productivity there, as well.
Let's call it 1.5x again, just for the sake of modelling.

This means that by improving yourself and committing to work a frankly
unreasonable 60-70 hours a week, you may get to 2.25x as much output compared
to "junior dev you" working 40 hours a week.

But note that neither of these is sustainable. You probably won't have time to
maintain that level of work and study for your entire career.

## Onboarding and return on investment

In contrast, consider what happens when you onboard a junior and mentor them.

Let's make up some numbers, assuming a new junior_with _no prior work
experience_:

- Onboarding a junior will take 50% of your working time for the first 1 month,
    after which time they'll be 50% as productive as you are normally.
- For the next 2 months, mentoring will take 25% of your time, after which
    they'll be 75% as effective as you are.
- For the next 3 months, you'll spend 10% of your time mentoring, after which
    your junior will be 90% as effective as you are normally.

> TODO(msuriar): make a graph.

You take a 50% hit in productivity in your first month, after which you have
gained 75% back (50% from your junior, 25% from reduced mentoring time), so
your total output (assuming everyone is working normal hours) is 1.25x your
base output working alone.

After another two months, you are at 1.65x your base output.

At the end of a six month onboarding period, you're at 1.9x your base output,
and no-one has had to work themselves into the ground.

These numbers are obviously a vast oversimplification, and they will be even
less representative for juniors with existing work experience. Juniors with
more experience will likely be more productive more quickly - and will likely
require less mentoring time invested.

We should also mention that sometimes mentoring may not be sufficient. It's
possible that new joiners are unable to make progress through your normal
onboarding/mentoring process. It's important as a mentor to recognise when this
is happening, take a step back and adjust the overall approach in concert with
your mentee and their manager. This gets more into people and performance
management - these details are probably for another post, written by someone
with more experience in these topics.

## Retention

One beneficial side effect of good mentoring is retention. Ongoing growth and
development, and new opportunities of ever increasing scope make teams with a
strong mentoring culture attractive for those looking to progress in their
careers. Through better retention, institutional knowledge is spread around and
remains with the team for longer.

This has implications for mentors: as junior mentees increase in skill and
independence, the role of mentors and managers is to continue to keep them
supplied with interesting and challenging work commensurate with their
capabilities.

## Sustainability

Assuming you have an environment that lends itself to new people learning and
becoming productive, and your staff turnover isn't too high relative to
onboarding time: in the medium term, onboarding and mentoring junior engineers
is the **single most impactful thing** you can do to improve the output of your
team.

# Onboarding advice

Some advice - a combination of what I've seen work before along with points
raised by Steven and Niklas in our discussion.

## Have a plan for onboarding

When people first join a team or company, they will often need a lot of
direction.

Have rough goals for what you want to teach and what you want your mentees to
achieve in their first day/week/month.

One thing that's useful is to have a "first change" that every new starter
does. This gives them a way to confirm that their development environment and
credentials work. It doesn't need to be anything core to the product - it could
be something as simple as adding their name to the `README.md` of the team's
repo.

After that, it's often useful to have a couple of lists of starter projects in
your backlog, for example "first week tasks" and "first month tasks". These
should be issues which:

- Are limited in scope.
- Support learning/exploring about your systems.
- Are not time critical.

As your mentee works through these issues, they will gain familiarity and
confidence with your team, systems and processes. In SRE teams I've worked on,
monitoring or rollout improvements often fall nicely into this bucket. In a
development team, you might consider localisation bugs, improved debuggability,
or reduced log spam. TODO(better examples?)

## Set expectations

It's important that your juniors acclimate to the way your team works. This
can cover a great deal - it will vary by team. But for some examples:

- How does your development process work?
- What is your branching strategy?
- Do you do pull requests and code review?
  - If so, what's the etiquette for getting someone to look at your changes?
  - What's a reasonable amount of time to wait for a code review?
- What are your policies around testing?
- ...

There are also simple expectations to set around their own learning experience.
A common pitfall for new joiners, particularly those who are shy or insecure or
worried, is to take to long to ask for help. It is ~never useful to have a new
team member banging their head in frustration and unable to progress. It's bad
for their morale, and its a waste of time.

I tend to set a strict time limit - 15 minutes, or 30 minutes - after which my
mentees should actively bother me if they're blocked. If you're in a physical
office, it's easier to load balance this among the people sat around. When one
or more people are remote, it's useful to have a named person to handle such
questions, and to make it explicit that handling those questions is an
important part of their job which they'll make time for.

Note that such instances shouldn't just be "oh, here's how you do that, back to
work you go" - instead, they should be teaching and learning opportunities for
both sides. Some example questions and conversation points:

- Where would you think to go and look for more information?
  - What can we do to improve the discoverability of this information?
  - Oops, we don't have that written down. We should fix that.
- What have you tried so far?
  - What would you expect to happen here?
  - What does that error tell you?
      - Here's a trick for finding where that error is being thrown in code.
  - How could we fix that error to get to the next one?

## Lead by example

Culture is the set of behaviours that are rewarded and punished. As a mentor,
you're responsible for demonstrating what good behaviour looks like in your
team or company, and also demonstrating vulnerability to show that people won't
be punished for it.

- If you're asking your mentees to test all their code, make sure you do the
    same.
- If you expect fast turnaround on PRs, then don't keep your mentees (or
    anyone else!) waiting on theirs.
- When you don't know something and need to ask for help, do so publicly. Make
    it clear that not knowing something is nothing to be ashamed of.
- When you screw up, stick your hand up and admit it. Blameless culture is
    important and valuable, but it takes work.

On that last point: myths and legends of past outages are a great ice breaker.
They're generally entertaining and engaging, they teach people about systems
and failure modes, and the reinforce a culture of open communication. They also
provide a demonstration that honest mistakes don't get people fired.

Niklas mentioned a book here which might be useful, for people who are not
story tellers by inclination: [The Leader's Guide to Storytelling]. I've not
yet read it, but I've added it to my unread pile of shame. When I get round to
it, I'll post a review.

[The Leader's Guide to Storytelling]: https://www.amazon.co.uk/Leaders-Guide-Storytelling-Mastering-Discipline/dp/078797675X


## Actively check in on people

In addition to making it clear that mentees should reach out when they're
blocked, try and get a sense for how they're doing. If you're in the same
office space, watch out for them looking frustrated or confused. Doing this
remotely is harder - see the next section.

It's important that your mentees have protected time to talk about their
experience more broadly. How are things going? What's your overall impression
of the product/team/company? Is there anything you need help with outside of
your immediate work?

What form this takes can vary. If you're working in an office together, then it
may be as simple as a chat over the mid morning coffee/tea break.
Alternatively, some people like to schedule a standing 30 minute meeting once a
week. Find what works for you and your mentee.

## Remote onboarding

Remote onboarding brings its own significant challenges. Certain aspects of
onboarding and mentoring become much more important when you're not meeting
with your mentees in real life every day.

Good documentation, onboarding processes, and small tasks ready to go are of
paramount importance in a remote environment. The last thing a new hire wants
to feel is unproductive, idle or forgotten about. Make sure they have all the
IT access they need, a clear list of things to spend time on, and a backlog of
reading for when they're waiting on something.

Also: be prepared to invest significant time in your mentees when onboarding
remotely. Lacking the ability to physically see when they're getting frustrated
or confused, you must make time to actively ask questions.

To provide one example of what this might take: when I onboarded a new hire in
my last team, I scheduled a 30 minute 1:1 for us every morning for the first
month.  Those sessions were used for Q&A, pair programming, discussion,
lectures - whatever we needed to spend time on. After the first month, those
meetings were dropped to 1 hour twice a week. We maintained a shared document
of notes for topics we wanted to cover in future sessions. As they became more
confident working indepently, the frequency of those sessions reduced until we
fell into a regular weekly cadences for 1:1s.

## Building a self sustaining culture

Another thing to bear in mind: if you're in a team or company that has constant
growth (even if it's only a new person every couple of months or so), you have
the opportunity to build a self sustaining culture which can improve your
team's shared understanding as well as provide your mentees with a support
structure beyond yourself.

In many of my teams, one of the first things a new joiner does in week one is
get a introductory/101 talk on the team and the systems their responsible for.
If this is in an office, it's typically done in an informal setting around a
whiteboard, rather than from pre-canned slides. The goal is to provide a simple
mental model of the service[^models] which everyone on the team can reproduce
from memory, with a one sentence description of what each box does.  Often, the
first instance of the talk for a new joiner is given by the last person to join
the team.

The diagram used in that one hour overview is repeated a lot in future, more
detailed training, with focuses on different subsystems and components.

When it comes to questions which people are blocked on, try and encourage
public discussion. That might be via voice in an office (be careful to respect
your co-workers' feelings on noise), or it might be in a team channel in Slack
or IRC. Putting those questions in a public forum makes them visible to other
people - sometimes one new joiner may be able to help another, which will
increase both of their confidence. This also relates back to setting an
example: as an experienced member of your team, make sure to ask questions
publicly when you run into something you don't understand. Demonstrate that a
lack of knowledge is nothing to be ashamed of.

[^models]: Diagrams as shared mental models are an extremely underused tool. I
  have opinions which I will hopefully express in another blog post and/or
  future conference talks.

# Conclusion

There is a some finite limit that you can achieve working as an individual. If
you're happy with tasks which fit within that limit, then that's fine - this
advice isn't for you.

If you want to be able to achieve and deliver more, the only way you'll be able
to do so is to **enable other people to help you**.

Mentoring is a long term strategy: make the time investment now, and you'll
reap the rewards later.

Productivity aside: mentoring can also be extremely rewarding. Seeing people I
mentored succeed and surpass me never fails to put a smile on my face.

If I'm remembered for nothing else than "Murali helped me get a good start",
that wouldn't be a bad outcome at all.

[CodingBlocks]: https://www.codingblocks.net/
[Slack]: https://www.codingblocks.net/slack
