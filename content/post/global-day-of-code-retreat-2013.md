+++
title = "Global Day of Code Retreat 2013"
date = 2013-12-15T00:12:00Z
updated = 2013-12-15T00:17:08Z
tags = ["learning", "coding"]
+++

Have you heard of Coderetreats? I hadn’t until a couple of weeks ago, when a
friend of mine prompted me to try one.  From the [About
page](http://coderetreat.org/about):

> Coderetreat is a day-long, intensive practice event, focusing on the
> fundamentals of software development and design. By providing developers the
> opportunity to take part in focused practice, away from the pressures of
> ‘getting things done’, the coderetreat format has proven itself to be a
> highly effective means of skill improvement. Practising the basic principles
> of modular and object-oriented design, developers can improve their ability
> to write code that minimizes the cost of change over time.

I signed up for the Global Day of Coderetreat session and then promptly forgot
about it until yesterday, when I realised that I was going to regret my
0600-1200 sleeping pattern today. (Other than making it difficult to interact
with normal people, however, it’s awesome.)

Despite my lack of sleep I dragged myself to the depths of Shoreditch where
[Unruly Media](http://unrulymedia.com/), our hosts for the day, make their
home.

To summarise the
[format](http://coderetreat.org/facilitating/structure-of-a-coderetreat), the
day is broken up into multiple 45 minute sessions. All sessions revolve around
pair programming a solution to [Conway’s Game of
Life](http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life). There is a
preference for using test driven development, and a hard rule that all code
must be deleted at the end of each 45 minute session.

We rotated pairing partners throughout the day, and there were no language
restrictions other than the basic sanity check of “at least one of the pair
should be comfortable with the language you’re using”. I only wrote in Python
and Go, since my recent programming experience is limited to those languages,
but people worked in Java, Javascript, Haskell and (I think) a couple of other
languages as well.

In addition to the basic rules, some of the iterations had constraints placed
on them, in order to stimulate thought about the problem domain or just for the
entertainment of the facilitators. (Mute immutability springs to mind. :P)

Was it worth it? I certainly think so. A day of practice with no pressure to
deliver useful code to a customer was very refreshing; I felt it enabled me to
think about the actual problem being solved as well as the various approaches
that could be taken. The enforced deletion of code as well as changing pairing
partners during the day lead me down several different tracks of how to solve
this problem.

The day was concluded with a wrap up, where everyone was asked three questions:

1. What, if anything, surprised you today?
2. What, if anything, did you learn today?
3. What, if anything, will you do differently in the future?

Speaking for myself:

I was surprised at how effective pair programming is. This was my first
experience with it, and I was expecting it to be stifling (having to wait for
other people, not using my own development environment). It transpired that
these worries were largely unfounded (Alex’s shocking vim skills not
withstanding; he has been directed to the [appropriate](http://vimcasts.org/)
learning resources). Having someone to bounce ideas off meant that at no point
did I ever get stuck; if I got to a point where I wasn’t sure what to code
next, my partner generally had a concrete suggestion. If neither of us were
quite sure how to progress, 30 to 60 seconds of chat usually resolved this and
we were back to making forward progress.

In terms of learning: I think for me, the biggest lessons were simply in
collaborative coding. As a network engineer, most of my coding since leaving
university has been knocking together code that just about works to solve
specific problems, with a couple of exceptions where I’ve worked with full time
developers on specific projects.

Things I’ll do differently: I will try and pair program more, when given the
opportunity; today has definitely given me an appreciation for its benefits. It
also brought home the real benefits of test driven development; one of the
exercises had the stipulation that one partner wrote the test code, and the
other partner the implementation. This turned out to be a good way of forcing
clarity of requirements, and having the tests in place gave me much greater
confidence when making changes, which was particularly important given the time
pressure.

Overall a very enjoyable and productive day; for those looking to practice the
art of programming, you could certainly do worse than attending a code retreat.
Many thanks to [Rachel Davies](https://twitter.com/rachelcdavies) for
organising the event and keeping us fed and watered, and to [Steve
Tooke](http://kickstartacademy.io/team#tooky) for facilitating. I look forward
to next year.
