+++
title = "Multiple SSH identities - beware!"
date = 2010-07-27T12:08:00Z
updated = 2011-04-22T20:41:37Z
tags = ["ssh", "authentication", "ssh-agent"]
+++

One of my jobs required me to make use of a number of SSH keys for access to
various different systems. This was a new experience for me; previous employers
as well as university had made extensive use of Kerberos/GSSAPI. I modified my
shell's startup fies to load the appropriate keys into my agent using
`ssh-add` on login, and promptly forgot all about it.

### The Symptoms

This worked seamlessly for some time, with me adding keys to
the list as I was granted access to new systems. However I noticed that after a
while, I would get `Permission denied (publickey)` errors when logging in to
some systems. It was pretty random, and I found that `ssh-add -D` (to remove
all my keys from my agent) followed by adding in the one I needed worked around
the issue. This happened rarely enough that I was never motivated to find the
underlying root cause.

### The Cause

It took me a long while to realise why this was the case; it was specifically
called out in [SSH: The Definitive Guide](http://amzn.com/0596008953). As an
aside: this is an excellent resource on the SSH protocol, covering the details
of both [OpenSSH](http://www.openssh.com/) as well as
[Tectia](http://www.ssh.com/) (the company formerly known as SSH Communications
Security, who developed the original SSHv1 protocol).

When you connect to an SSH server specifying publickey authentication, unless
there is a specific identity configured via the `IdentityFile` option, SSH will
attempt to use each key loaded into your ssh-agent in order.

OpenSSH's sshd supports a keyword `MaxAuthTries` which is particularly relevant
here To quote the [man
page](http://www.openbsd.org/cgi-bin/man.cgi?query=sshd_config&sektion=5):

```manpage
MaxAuthTries
             Specifies the maximum number of authentication attempts permitted
             per connection.  Once the number of failures reaches half this
             value, additional failures are logged.  The default is 6.
```

Each key that is presented for authentication is counted as a failed attempt.
Thus, with the remote sshd using a default value of `MaxAuthTries 6`, if the
key required to log in is the 7th one loaded into your agent, you will receive
a `'Permission denied (publickey)'` and your connection attempt will fail.

Note that this also impacts login attempts to servers requiring
keyboard-interactive/password authentication, if you're not careful. Looking at
the
[ssh_config](http://www.openbsd.org/cgi-bin/man.cgi?query=ssh_config&sektion=5)
man page:

```manpage
PreferredAuthentications
             Specifies the order in which the client should try protocol 2
             authentication methods.  This allows a client to prefer one
             method (e.g. keyboard-interactive) over another method (e.g.
             password).  The default is:

                   gssapi-with-mic,hostbased,publickey,
                   keyboard-interactive,password
```

Note the default: publickey authentication is attempted prior to
keyboard-interactive or password. If there are 6 or more keys loaded in your
SSH agent, they will be presented in sequence until the server's `MaxAuthTries`
value is reached, and the connection will be closed.

### Workarounds

If you require more than 6 keys loaded into your SSH agent, or access to
publickey and password based systems, there are workarounds. You can specific
authentication methods and identity files either on the command line or in your
`ssh_config` files. Some common patterns below:

#### ssh_config

`ssh_config` allows you to specify keywords on a per host basis, as well as
using limited globbing.

For example, to specify authentication methods and identities for specific
hosts, with a fallback of Kerberos for any hosts you haven't called out
specifically, try the following:

```
# For badger, only try a password, don't bother with public keys.
Host badger
  PreferredAuthentications password

# For serenity, use publickey authentication and use the identity in
# ~/.ssh/keys/browncoat
Host serenity
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/keys/browncoat

# For miranda, use publickey authentication and use the identity in
# ~/.ssh/keys/reaver
Host miranda
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/keys/reaver

# For any other hosts, only attempt gssapi-with-mic (aka Kerberos)
# authentication
Host *
  PreferredAuthentications gssapi-with-mic

```

#### Command line

The keywords mentioned above can also be specified on the command line with the
use of the `-o` command line option to SSH. Multiple keywords can be specified
with multiple `-o` arguments. For example:

```
user@host$ ssh miranda -o "PreferredAuthentications=publickey" -o "IdentityFile=~/.ssh/keys/reaver"
```

OpenSSH has a vast number of configuration options and features which are of
use in different scenarios. I may do another post on this in the future, but in
the interim, if you're interested, the OpenSSH man pages or the book referenced
above are well worth reading.
