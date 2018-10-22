+++
title = "Lab machines on the cheap"
date = 2009-12-11T01:37:00Z
updated = 2013-12-03T02:48:13Z
tags = ["vmware", "lab", "freebsd"]
+++

While my Linux/UNIX skills as a user are fairly good, my administration skills
are virtually non-existent. I know how to `su -` and `apt-get`, but installing
a production system from scratch and understanding how to put a secure, stable
OS build together are well beyond my ken. My BSD knowledge is... limited, to
say the least.

Fortunately, all of the above lend themselves quite neatly to the use of
virtual machines; there's a lot to be said for being able to install a fresh
operating system into a clean environment, play with it and break it to your
heart's content and then wipe the slate clean when you're finished. All this,
and no extra costs for new hardware either. [VMWare
Server](http://www.vmware.com/products/server/) is freely available for
download, and allows you to run virtual machines in containers on top of your
existing Linux/Windows install.

After the installer finished I was prompted with a web GUI to log in to,
through which I could manage the virtual machines running on my desktop.
Somewhat obscurely, I was prompted for a username and password to log in to
VMWare Infrastructure Web Access, despite not having been prompted to configure
one during the installation process. A quick web search revealed that VMWare
authenticates against the local OS; once I had input my credentials and
installed the requisite Firefox plug-in, I was presented with an inventory:

{{< figure
link="http://1.bp.blogspot.com/_U_HD6hw90dM/SyGfNU9cetI/AAAAAAAAIOA/Q9DKaawRrqQ/s1600-h/vmware.png"
src="http://1.bp.blogspot.com/_U_HD6hw90dM/SyGfNU9cetI/AAAAAAAAIOA/Q9DKaawRrqQ/s320/vmware.png"
>}}

Half an hour later, I had a functioning [FreeBSD](http://www.freebsd.org/)
installation on a newly created VM:

{{< figure
link="http://2.bp.blogspot.com/_U_HD6hw90dM/SyGfTEEO8ZI/AAAAAAAAIOI/sMDgvL2S9ZY/s1600-h/bsd.png"
src="http://2.bp.blogspot.com/_U_HD6hw90dM/SyGfTEEO8ZI/AAAAAAAAIOI/sMDgvL2S9ZY/s320/bsd.png"
>}}

The plan is to start by using pre-configured installations of BSD, Solaris and
Linux, and to move towards my own hand-crafted builds. The end goal is to have
minimal, secure images for each OS that perform specific roles within an IT
infrastructure. The first two on the list are a DNS server and a Kerberos
KDC/TGS.
