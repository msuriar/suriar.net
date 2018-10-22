+++
title = "GNU Make: a very short introduction"
date = 2009-12-11T02:48:00Z
updated = 2013-12-03T02:52:02Z
tags = ["make", "config-management", "automation"]
+++

I finally got sick and tired of having a slightly different command line
environment in each of the Linux/UNIX environments I use. This led me to put
some effort into collating my various dotfiles (for bash, vim and screen) and
coming up with a consolidated version of each, tracked using git, that I could
install on any machine which I had a unique home directory on and not have to
remember which of my preferences I've configured on this particular machine. At
some point, I may post annotated version of said files here, along with what
lead me to implement the various things in each file.

Today's post, however, is to do with a tool called
[`make`](http://www.gnu.org/software/make/). Make is most commonly associated
with managing software builds on UNIX/Linux platforms, but more generically it
can be described as a dependency tracking engine. A 'Makefile' consists of a
set of rules of the form:

```makefile
<target1> [target2 ...]: [dependency(ies)]
    [command 1]
    [command 2]
    ...
```

Where 'targets' are files that are being generated, 'dependencies' are other
files (or other make targets) on which the current target depends, and
'commands' are commands to execute in order to generate an up to date version
of the current 'target'.

In the case of my dotfiles, I wished to be able to do a few things without retyping excessive numbers of commands:

1. For each of the three subdirectories in my 'dotfiles' project, create a gzip compressed TAR archive, named &lt;project&gt;.tgz.
2. Additionally, my 'vim' directory contains several colour scheme definitions. I want two versions of my 'vim' archive, with and without the 'colors' directory.
3. I would like the ability to automatically copy these files to a directory accessible via HTTP, allowing me easy access to them wherever I have internet access.
4. I would like the ability to delete all existing archives in the project directory.

## The naive way
So, let's assume that I don't have make available - how would I go about each of these tasks?

Creating archives (ignoring any advanced shell features, for simplicity):

```
[muz@codd:dotfiles]$ l
bash/ screen/ vim/
[muz@codd:dotfiles]$ tar -czf bash.tgz bash/
[muz@codd:dotfiles]$ tar -czf screen.tgz screen/
[muz@codd:dotfiles]$ tar -czf vim.tgz vim/
[muz@codd:dotfiles]$ l 
bash/ bash.tgz screen/ screen.tgz vim/ vim.tgz
[muz@codd:dotfiles]$
```

Creating separate archives for vim with and without colors:
```
[muz@codd:dotfiles]$ ls vim/
colors/ vimrc
[muz@codd:dotfiles]$ tar -cvzf vim-no-colors.tgz vim/ --exclude=colors
vim/
vim/vimrc
[muz@codd:dotfiles]$
```

Copying to a directory:
```
[muz@codd:dotfiles]$ cp *.tgz ~/public_html/dotfiles/
[muz@codd:dotfiles]$ l !$
l ~/public_html/dotfiles/
bash.tgz  screen.tgz  vim-no-colors.tgz  vim.tgz
[muz@codd:dotfiles]$
```

Removing all the tars:
```
[muz@codd:dotfiles]$ l
bash/  bash.tgz  screen/  screen.tgz  vim/  vim-no-colors.tgz  vim.tgz
[muz@codd:dotfiles]$ rm ./*.tgz
[muz@codd:dotfiles]$ l
bash/  screen/  vim/
[muz@codd:dotfiles]$
```

### The *make* way
Based on my initial requirements, this was my initial makefile:
```makefile
all: bash.tgz screen.tgz vim.tgz vim-all.tgz
clean:
        rm -f *.tgz
bash.tgz: ./bash/*
        tar -czf bash.tgz ./bash/
screen.tgz: ./screen/*
        tar -czf screen.tgz ./screen/
vim.tgz: ./vim/*
        tar -czf vim.tgz ./vim/ --exclude=colors
vim-all.tgz: ./vim/*
        tar -czf vim-all.tgz ./vim/
publish: all
        cp *.tgz ~/public_html/dotfiles/
        chmod go+r ~/public_html/dotfiles/*.tgz
```

This does everything that I initially specified:

* make all
  * Create four .tgz files; bash, screen, vim, and vim-all, if any of the files
    that an archive contains have changed. (I changed vim to mean vim without
    the colors directory.)
* make clean
  * Remove all files ending .tgz. This is an example of a '[phony
    target](http://www.gnu.org/software/automake/manual/make/Phony-Targets.html)';
    a make target that does not refer to a specific file, but instead causes a
    set of commands to be executed whenever it is invoked.
* make publish
  * Copy all `*.tgz` files in the project directory to a web directory, and
    make them readable.

However, this is fairly verbose, and there is a lot of repetition. Using some
variables/macros (some internal to make, some defined by me) my final make file
is a little less redundant:

```makefile
# Make file for tarring and publishing dotfiles.

HTDOCS=~/public_html/dotfiles
ARCHIVES=*.tgz
TARCMD=tar -czf

all: bash.tgz screen.tgz vim.tgz vim-all.tgz
clean:
        rm -f $(ARCHIVES)
bash.tgz: ./bash/*
        $(TARCMD) $@ ./bash/
screen.tgz: ./screen/*
        $(TARCMD) $@ ./screen/
vim.tgz: ./vim/*
        $(TARCMD) $@ ./vim/ --exclude=colors
vim-all.tgz: ./vim/*
        $(TARCMD) $@ ./vim/
publish: all
        cp $(ARCHIVES) $(HTDOCS)
        chmod go+r $(HTDOCS)/$(ARCHIVES)
```

(The in-built variable `@` refers to the name of the current target.)

Make can be used to simplify any procedure that requires multiple commands to
be run as various files change. Software builds, operating system configuration
files, document publishing... the list goes on. From a system/infrastructure
administration point of view, there is a strong argument to be made that a
makefile is the logical endstate of getting your documentation up to date.
Rather than having a document that a human operator follows, have a makefile
that a system can execute, and remove as much possibility for human error as
possible. (Look for another post on the subject of human error soon.)

To quote an anonymous system administrator: "Everything should be a Makefile."
