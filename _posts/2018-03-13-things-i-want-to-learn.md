---
layout: post
title: "Things I Want to Learn"
date: 2018-03-13
tags: [list, goals, learning]
comments: true
share: true
---

# Software

## Docker

* How do I set up my own Docker registry? (Dockerhub?)
* What's the difference between pulling an image and building it yourself?
* Do images have to be hosted somewhere online, or can they be local?
* How can I effectively use containers at work to make life easier?
  * DNS/DHCP?
  * Puppet/Foreman?
  * GitLab runners?
* Can containers fully replace infrastructure VMs that run headless, or even bare metal?

## Foreman

* How can I use Foreman to provision new nodes from inception to decommission?
* How can the use of environments, config groups and host groups be improved in our current implementation?

## Ansible

* What does Ansible offer that I can't do with Puppet already? Does it solve any obvious problems?

## HTML/CSS

* How does Jekyll work? How can I customize the layout of my blog using templates and layout hacks?
* Is SASS compiled? If I change a variable in one place, will the change be reflected through the blog immediately or does it need to be recompiled? 
* What does the `jekyll build` command do? 
* How does git know that I committed a change to the blog and regenerate the content on the web?

# Computer Hardware

* What is iSCSI and how are we currently using it in our environment?
* What's the big difference between a SAN and a NAS? (I think I kind of know this already, but I'd like to know more)
* What are some alternatives to NFS storage?
* What do the stats of hard drives refer to (IOPS, read/write, etc.) and how do they affect the decision making process when shopping for new hardware?
* What is a fabric extender, and how does it relate to a switch?

# Electronics

* How do I solder something? Pitfalls to avoid?
* What is a transistor? Capacitor? What role do they play in computers? In guitar amplifiers?
* What can a multimeter be used for?

---

# Docker

I hear about Docker constantly. We use it at work for Continuous Integration, and I basically get it. Spinning up a fresh container ensures that we have our dependencies completely defined, there aren't any weird config options hiding in the shadows, and everything is basically reproducible if needed. That (pretty much) makes sense to me for that particular use case. But when people start talking about using Docker for system admin stuff, then my ears really perk up. I can't really mentally grasp the benefit of configuring a Docker container to do something like DNS. Our DNS in particular is pretty much always changing. Entries are added (manually, or in some environments dynamically) and the files are constantly being edited. New zones are added. So how does a container really help in that case? 

I can think of a few benefits it might provide. Say we need to migrate to a new operating system on the host. CentOS 6 is going out of support in a few years, so we want to move to 7. That process would be a lot easier if we could just reload the host, pull the image and say "Go!". 

But then that just brings up more questions:

* How do we inject DNS config into a container? Have it pull down a git repo?
* How does IP configuration work? Does it matter?
* Can we get Puppet running automatically during the container initialization? If not, how do we ensure needed services are running?
* Once a container is exited, does it just sit somewhere and take up space until we go manually delete it? Can old containers somehow be rotated or expired?

Obviously there are a lot of considerations that go into a big infrastructure switch like this, but I'd really like to try and figure out a way to do this ourselves. This is one of my biggest and most pressing goals.

# HTML/CSS

I really like web design as a casual hobby. It's fun for me. There's something really cool about the instant results you get when you can refresh a page that you wrote.

Jekyll is really cool for quickly spinning up a blog, and using someone's existing template, and having it "just work" but I'd really like to know a little more about its inner workings. How are the directories organized and what goes where? 

At some point I'd definitely like to design my own Jekyll-powered blog and make it uniquely me. I love the layout of Julia Evans' blog. It's really clean and nice to look at.

# Computer Hardware

This is the next-level system admin stuff that I really want to dive into. Everybody has to start with the outer layer of systems; your services, end-user workstations, visible switches. And I have a good idea of how most of that stuff is networked and how it works. But I really want to dig in to the deeper, gnarly fiber switches and what really makes them work. How they're connected to each other, and how the hardware could be improved in the future.

---

While I was writing my list above, I couldn't help but notice that it was starting to read like a quiz I might've taken in college. It's funny how your perspective on things can change. As a student, you intrinsically resist open form questions that force you to acknowledge what you've learned. Or I did anyway, especially toward the start of my college career. That's probably why it took me so long to finish. But I noticed that by the end of college, when I'd finally found the subject that I actually wanted to study and was taking classes that I felt were relevant to my future career and my life, answering this questions at the end of every chapter became more of a labor of love. There was no longer any urge to get through them as fast as possible and just reword the definitions in the book. Even with this list that I've written for myself (as daunting as it may be), the object isn't to just type those queries into a search engine and answer them so they're done. The idea is to **learn** and be able to answer them comfortably in my own words. And then put that knowledge into practice in a real-world setting. 

I feel lucky because I landed in a career where I want to improve myself. There's an urge for me to learn about the things that I feel can make me valuable as an employee and as a person, and writing down the list makes it much more manageable. I hope in the future I'll be able to repost this list with a few of the items struck through. What does your list look like?
