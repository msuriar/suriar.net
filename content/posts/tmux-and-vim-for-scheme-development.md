+++
title = "tmux and vim for Scheme development"
date = 2013-12-03T13:48:00Z
updated = 2013-12-03T16:41:24Z
tags = ["productivity", "vim", "tmux"]
+++

I’m currently working my way through [Structure and Interpretation of Computer
Programs](http://mitpress.mit.edu/sicp/), as a lot of people I know speak very
highly of it as a text on the craft of programming.

It uses the
[Scheme](http://en.wikipedia.org/wiki/Scheme_%28programming_language%29)
programming language, a dialect of
[Lisp](http://en.wikipedia.org/wiki/Lisp_%28programming_language%29). Most
tools for developing in Lisp and Scheme (e.g.
[SLIME](http://www.common-lisp.net/project/slime/)) are based around the
[`emacs`](https://www.gnu.org/software/emacs/) text editor.
Without going down the [rabbit hole](http://en.wikipedia.org/wiki/Editor_war),
suffice it to say that I’m a [`vim`](http://www.vim.org) user, and wanted an
alternative.

My first thought was to use
[slimv.vim](https://github.com/vim-scripts/slimv.vim), which apparently
provides `vim` users with similar functionality to SLIME.  Unfortunately, my
attempts to get slimv.vim to play nice with
[`mit-scheme`](http://www.gnu.org/software/mit-scheme/) on OSX lead to
frustration; I may revisit that at a later date, but in the mean time, I needed
to get something working.

Enter [`tmux`](http://tmux.sf.net/). `tmux` is a terminal multiplexer in the
vein of GNU [`screen`](http://www.gnu.org/software/screen/). One of its main
benefits over `screen` is that almost every action in `tmux` is scriptable via
the command line, which lends itself nicely to automation and integration with
other programs.

For example, we can load the contents of a file into a `tmux` paste buffer…

```
$ tmux loadb <filename>
```

… and then paste the contents of a paste buffer into a specific pane in `tmux`.

```
$ tmux pasteb [-t [<session>:]window[.<pane>]]
```

If a target isn’t specified, the currently active pane is used. Windows can be
specified by name or by number.

To create a new pane:

```
$ tmux splitw [-h] [cmd args...]
```

(The `-h` creates a side-by-side split; by default, `tmux` splits are stacked
vertically.) Finally, we can create a mapping in `vim`, which saves the current
file and then shells out to `tmux`, reads in the contents of the current file,
and pastes it into a target pane.

```
:map ,t :w \| !tmux loadb % && tmux pasteb -t 0.1
```

I can now have a scheme REPL side-by-side with my editor, and whenever I hit
`,t` my code will be saved and run through the REPL.

A quick screencast to demonstrate:
{{< youtube Va36_6xYvt0 >}}
