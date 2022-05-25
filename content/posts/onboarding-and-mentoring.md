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
combined with some good advice from Niklas Asp.

[Steven Leadbeater]: https://www.linkedin.com/in/steve-leadbeater/

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

Let's make up some simple assumptions:

- Onboarding a junior will take 50% of your working time for the first 1 month,
    after which time they'll be 50% as productive as you are normally.
- For the next 2 months, mentoring will take 25% of your time, after which
    they'll be 75% as effective as you are.
- For the next 3 months, you'll spend 10% of your time mentoring, after which
    your junior will be 90% as effective as you are normally.

TODO(msuriar): make a graph.

You take a 50% hit in productivity in your first month, after which you have
gained 75% back (50% from your junior, 25% from reduced mentoring time), so
your total output (assuming everyone is working normal hours) is 1.25x your
base output working alone.

After another two months, you are at 1.65x your base output.

At the end of a six month onboarding period, you're at 1.9x your base output,
and no-one has had to work themselves into the ground.

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
mentees should actively bother me if they're blocked. Note that such instances
shouldn't just be "oh, here's how you do that, back to work you go" - instead,
they should be teaching and learning opportunities for both sides. Some example
questions and conversation points:

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


## Make time to check in on people

## Remote onboarding

## Building a self sustaining culture

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
