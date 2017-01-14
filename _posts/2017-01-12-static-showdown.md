---
layout: post
title: "Static Showdown"
description: "The trials and tribulations of spinning up a blog without WordPress."
date: 2017-01-12
tags: [rant, blog]
comments: true
share: true
---

## Static Site Generators

Static site generators are pretty cool. No databases to maintain, you can host a site on GitHub Pages for free, and you feel like you have more of a say in the display, layout, and content of your website. I'm not exactly sure how it works; basically, you write up a markdown file using the text editor of your choice, those markdown files are parsed and used as variables in template files, all of the code is "compiled" into HTML and new files are generated; those files appear in their correct locatins in your repo. Commit and you are set. 

That said, like most things created by and for the technologically-inclined, there are a lot of options. Possibly too many options. Which commonly leads to the phenomenon known as **analysis paralysis**. Some are written in Python; some in Ruby. Some are easy and come with quickstart binaries that configure everything for you, some are bare bones and don't offer guidance. But the benefit of options is full control and the opportunity to customize and hack. That can be worth a lot.

I attempted to use [Jekyll](http://jekyllrb.com) a year or two ago with poor results. I was pretty happy with my cliche Bootstrap landing page, so the blog wasn't a big priority and I kind of forgot about integrating it. Sometime last week I realized that I'm not a web designer or developer, I don't really have a portfolio, and I don't have a need for a fancy single page portfolio-centric webpage. I also realized that *I've got some things to say*. The statically generated dream was alive again.

## Ruhoh

Jekyll was off the table - I'd failed previously and felt burned by my assumed incompetence. So it turns out the author of Jekyll Bootstrap also created their own generator called Ruhoh, which to me sounds like the sound Scooby Doo makes when he and Shaggy just ran into the monster of the haunted amusement park. Most of the apps I've used that are built with and use Ruby seem to work pretty well (looking at you, [Foreman](http://theforeman.org)), so I wasn't hesitant to jump in. Python, on the other hand, is almost always a complete nightmare to use. More on that later. All went well with Ruhoh at first, I had the built-in web server running fine, when suddenly the entire thing crashed with some kind of rvm error. This was enough to make me say goodbye - I know there is some involvement in doing something like this, but debugging a bunch of Ruby stuff was not in my agenda for something that was supposed to be a fun project. Enter Pelican.

## Pelican

Realizing Ruhoh wasn't even in most people's top ten lists of this sort of tool, I took to Google to find what the people loved. I found a blog entry by someone who spoke highly of a generator called Pelican. I noticed it Python-based, which made me wince a bit. I don't know if everyone feels the way I do about Python, but it's been nothing but a huge pain for me since college. 

### Quick Rant about Python

I am generally an Apple guy. I really like OS X (my feelings on macOS are ambivalent at best) because it's kind of the perfect marriage of a well-established OS like Windows with the organization and tweakability of UNIX systems. It works well for me. Last year during my senior year I took a course about Database Design. Our professor was hell bent on not only educating the class on SQL statements, but using LAMP and Python to send database queries through a website. I get it, I guess, it's kind of applicable, but the library to do all that required Python 3. System python, if I'm not mistaken, is 2.7, and I'm also assuming that changing the default will screw stuff up. I'm paranoid. Anyway, after weeks of fiddling with multiple versions of Python living peacefully with each other (spoiler alert: they didn't) I passed the course. My MacBook, unforunately, is now in a weird purgatory state with Python and I'm frankly a little terrified to change anything for fear of irreparably breaking something.

### Back to Pelican

Despite my reservations, I persisted with Pelican. The installer was actually great - pip makes everything really easy. They even include a pelican-quickstart binary that guides you through everything. I was pretty pleased with Pelican. Unfortunately, the local server wouldn't start and I abandoned Pelican like a coward.

## Enter Jekyll...again

Why mess with the best? Jekyll is the first tool I'd ever heard of that does this sort of thing. Maybe it's not so bad.

Turns out it's awesome! Getting started is as easy as cloning a git repo. I think my main issue is that last year I was using the Git GUI client, which is in every case a mistake. After working in our lab for almost a year, I have a pretty decent grip on git in the command line (even branches!) so this was pretty trivial. Anyway, I downloaded a theme and off I went.

## The Blog

If you're reading this probably anywhere close to the time of writing, this blog is probably full of default boilerplate stuff. It's taking forever to get everything fully customizable. But, at the same time, it's pretty awesome the way it all works. You basically put some vital information into a config file and it uses those as variables across the entire site during compilation, resulting in a fast deployment. Vim, it turns out, isn't the *best* when it comes to writing big posts like this, but it seems to be doing okay. It's certainly better than WordPress. Changing fonts was a breeze, since the theme I chose already had a Google Fonts declaration hidden in the SASS files. It's been pretty great so far.

## The Future

I haven't successfully blogged since the days of Xanga. Yeah - Xanga. I will check back in as needed and hopefully have some good thoughts to document.  
