---
title: "The Case of the Missing Interface"
date: 2018-03-28
tags: [linux, sysadmin]
categories: [sysadmin]
---

I had an interesting request at work yesterday afternoon that I thought might be worth reflecting on for a bit. It involves a laptop restored from an image of a different laptop (with potentially different hardware), no internet connection, and a hacky **systemd** solution at the end.

<!--description-->

# Background Info

We'd set up a laptop a few months ago with Ubuntu 16.04 and some special configs for security as well as heavy graphics-related stuff for one of the tasks our group supports. I'm pretty sure it was a System76. The team needed another identical laptop, and figured that they could just create and restore an image from the original laptop. I can't really say if the new one was identical in terms of hardware, but I now suspect that it was not, based on the pretty much immediate problems they ran into before calling us. It looked like an Origin, or possibly some kind of clone; I don't think there was any branding on it at all. There were, of course, no drivers included with the laptop and a pretty much non-existant tech support department.

# The Problem

It's pretty straightforward - the OS couldn't identify the Ethernet interface. Running `ifconfig` only showed the loopback interface (`lo`) and nothing else. `lspci` did show a generic Atheros device connected, but it wasn't able to get any information about its model number. Before I got there, they'd sent me some output from running `lshw -C network`:

```bash
*-network UNCLAIMED
description: Ethernet Controller
product: Qualcomm Atheros
vendor: Qualcomm Atheros
physical id: 0
bus info: pci@000:6d:00.0
version: 10
width: 64 bits
clock: 33MHz
capabilities: pm pciexpress msi msix busmaster cap_list
configuration: latency=0
resources" memory:de300000-de33ffff ioport:d000(size=128)
```

What does this mean? Seems pretty obviously to be a driver issue, but why is it happening? 

# Findings

This is why I suspect the new laptop has different hardware - maybe the interface is too new for an old LTS release of Ubuntu to support? Or maybe it's something specific to Atheros interfaces? Either way, it's only a problem on this laptop - the System76 (which uses an Intel interface) seems to be fine. And what sometimes sucks about old/extra stable OS releases (like CentOS, for instance) is that new hardware (or more unusual choices like Atheros) will really screw it up. Most of the forum posts I found regarding this issue were either from 2012 or just told me to upgrade to a much newer kernel/version of Ubuntu, which in this case wasn't really an option. I finally found an [askubuntu thread](https://askubuntu.com/questions/881479/install-atheros-e2600-drivers) with an answer that recommended the following commands:

```bash
sudo modprobe alx
echo '1969 e0b1' | sudo tee /sys/bus/pci/drivers/alx/new_id
```

I'm usually pretty cautious about randomly executing commands that I don't really understand the purpose of (what is `tee`?) but 3 upvotes on the answer and some slight desperation made me do it anyway. Incredibly, it worked. `ifconfig` showed a new interface that was good to go. After solving some unrelated DHCP issues, the laptop was online. But I couldn't help but wonder how and why it worked. Also, the changes were lost as soon as the machine rebooted.

# Reflection

So the `modprobe` command is pretty straightforward - it just loads (or removes) a module to/from the kernel and takes the module name as an argument. And in this case that module was `alx`. I'd kind of cheated earlier by looking at the `lshw` output from the working laptop to see what driver it was using, and `alx` was the one. The documentation for that driver states that it's intended to "add support for newer Ethernet device," which lines up with our purpose. So the module is loaded in, that makes sense. But then what is this echo pipe tee business? What is that file that it's writing to?

**kernel.org** provides some interesting insight here:

> What:       /sys/bus/pci/drivers/.../new_id
> Date:       December 2003
> Contact:    linux-pci@vger.kernel.org
> Description:
>         Writing a device ID to this file will attempt to
>         dynamically add a new device ID to a PCI device driver.
>         This may allow the driver to support more hardware than
>         was included in the driver's static device ID support
>         table at compile time.

Hey, cool. That makes sense. The first part of the second command (the `1969 e0b1` part) was visible when running `lspci -nn` and indicates the interface's vendor and device IDs; I guess I got kind of lucky here. I don't know that I would've thought to change up the input of that command to make it match the vendor and device ID of the interface. Lucky coincidence.

So that's all good stuff, but what in the world is `tee`? I consider myself a fairly competent Linux user and I've never used this before. Well, here's a man page snippet:

> TEE(1)                        User Commands                          TEE(1)
> 
> NAME
>        tee - read from standard input and write to standard output and files

Huh. Basically, `tee` just passes output to stdout **and** a file. I don't know that using it is any different than just using `echo` again; maybe it's got something to do with the permissions or sudo? 

*a brief Google search occurs...*

[Yep!](https://stackoverflow.com/questions/84882/sudo-echo-something-etc-privilegedfile-doesnt-work-is-there-an-alterna) Turns out output direction (using the > operator) is performed by the shell and you'd have to wrap the entire command in a `sudo sh -c` before running it. The `tee` is actually pretty elegant in that way, and a similar usage is the top answer on stackoverflow. I'm guessing that's a special file since I couldn't `cat` it, even as root.

# What about the rebooting?

Right. When we reboot, all of our changes disappear. That symptom has me pretty much convinced that this solution is a total hack and is probably not the right way to do this, but it's been hours and it works. If it ain't broke, right?

My first instinct is to write up a little bash script and put it in `init.d` or something so it runs after the OS is up. A voice echoes through my head...

*init is dead... use __systemd__...*

Yeah, yeah. I know. I like systemd as much as the next guy (actually, probably more than the next guy since there's a large group of people who really hate it) but I've never gone through the process of writing my own service. It's actually pretty easy; it ended up looking something like this:

```bash
[Unit]
Description=Loads alx module and inserts new id's
Before=network

[Service]
Type=oneshot
ExecStart=/sbin/modprobe -q alx
ExecStart=/bin/bash -c "echo '1969 e0b1' > /sys/bus/pci/drivers/alx/new_id"

[Install]
WantedBy=multi-user.target
```

I shamelessly stole this from an [ArchLinux forum post](https://bbs.archlinux.org/viewtopic.php?id=207885) by a guy who was having the exact same issue as me, but it ended up working great. I now see that bash-shell-wrapping thing I mentioned earlier that makes the permissions for writing to that file work. I tried to dissect this as I typed it in, just for future reference. The `Before=network` line seems pretty clear; start this service before the `network` service starts. That's pretty cool. I guess you can do that with any service that needs the one you're writing as a dependency.

Then you just execute the commands you need and tell it which runlevel should use the service, and make sure the service is enabled so it starts on bootup. I think the people who don't like systemd are probably just incredibly used to doing things a certain way and hate change, because to me this seems great.

# Success

I have to admit that I was really proud of how this whole thing worked out. I came into this situation knowing pretty much nothing and left feeling not only accomplished, but like I'd learned something. I also got some useful reinforcement during the typing of this post while diving into a few questions that had lingered from yesterday. 

Also thinking of doing some blog layout changes tonight, let's hope it's not a total unreadable disaster.
