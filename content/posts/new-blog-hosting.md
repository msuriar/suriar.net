+++
title = "New blog hosting"
date = 2018-12-02T12:20:00Z
tags = ["blogging"]
+++

[Previously] I concluded that the thing that I most care about is the ability
to host content. I don't care about comments, nor do I care about a shiny
WYSIWYG editor - in fact, I've become habituated to writing plaintext MarkDown.
This led me to conclude that rather than using a platform like [Blogger], I
could likely get by with a static site generator. This has the additional
benefit of additional portability, since I just need someone who can host
static HTML.

After doing a brief bit of research, I settled on [Hugo] as my static site
generator of choice. This was mostly because the most common thing ([Jekyll])
is written in Ruby, which I have ~no experience with. [Pelican] was another
option, but I didn't see any strong argument in favour of one over the other,
so Hugo won due to [recommendations] from other people.

I then needed to migrate my existing content from Blogger into Hugo markdown.
This was a little annoying, but not disastrously so. I [downloaded] an XML
backup of my existing content from Blogger. I then used [blogimport][^1] to
generate Markdown from that XML backup. Here is where I ran into a couple of
issues.

Firstly: the import tool assumes that all timestamps in the XML export with
specify a [timezone offset].

    t, err := time.Parse("2006-01-02T15:04:05.000-07:00", v)

In my case, several of my blogs were written when my local timezone was UTC,
and hence the ISO8601 timestamp didn't include an offset. I worked around this
by adding `+00:00` to the timestamps in my exported data where appropriate,
using `sed`. So far so good.

The second issue I ran into: the Blogger WYSIWYG editor had embedded raw HTML
styling in the export of my blog. This raw HTML ended up in the converted
MarkDown. Worse, different blog posts had somehow ended up with different
styling for the same effect (e.g. `<b>` vs `<h1>` for headings).

Some stuff was trivially fixable - for example, it was easy enough to write a
`vim` substitution for links of the form:

    <a href="http://example.com/blah">blah</a>

And replace them with:

    [blah](http://example.com/blah)

However, for much of the styling, I ended up just hand editing/cleaning things
up by hand in VIM.

Once I had well formed Markdown, the next step was to get them being served up
in Hugo. Hugo's [getting started] documentation is pretty good. I also needed
to find a theme - there are [several themes] for blogs published on the
official site.

This is where Hugo's documentation fell down a bit, to be honest. It took some
work to find a minimalistic theme with pagination, and how to distinguish
between pages and blog posts; I ended up settling on this theme called [coder].

Finally, I wanted to find a hosting provider. For now I'm using [Netlify].
They're a general "host web projects" company. Nice things I've noticed:

- They have a free tier which allows custom domains.
- They [support TLS] with [Let's Encrypt] certificates
- They support automated deployments based on pushes to [Github]/[Bitbucket].

~~The one thing that they're missing is IPv6 support, which appears to be a
limitation of [DigitalOcean] (which is who Netlify use for hosting their VMs).
The lack of v6 is annoying, but not enough so to motivate me to move given
the other convenient features Netlify provide otherwise.~~

UPDATE: at some point in the last 18 months Netlify added IPv6 support. Win!

TODO(msuriar): at some point I should publish the repo for this blog, including
the Hugo config.

[Previously]: {{< relref "back-again" >}}
[Blogger]: https://www.blogger.com/
[downloaded]: https://support.google.com/blogger/answer/41387
[blogimport]: https://github.com/natefinch/blogimport
[timezone offset]: https://github.com/natefinch/blogimport/blob/master/main.go#L28
[Hugo]: https://gohugo.io/
[Pelican]: https://blog.getpelican.com/
[Jekyll]: https://jekyllrb.com/
[recommendations]: https://miek.nl/2016/february/20/hugo/
[getting started]: https://gohugo.io/getting-started/
[migration]: https://gohugo.io/tools/migrations/#blogger
[several themes]: https://themes.gohugo.io/tags/blog/
[coder]: https://github.com/luizdepra/hugo-coder
[Netlify]: https://netlify.com
[DigitalOcean]: https://www.digitalocean.com/
[support TLS]: https://www.netlify.com/docs/ssl/#netlify-certificates
[Let's Encrypt]: https://letsencrypt.org/
[Github]: https://github.com/
[Bitbucket]: https://bitbucket.org/

[^1]: There are import tools recommend on the Hugo [migration] page.
