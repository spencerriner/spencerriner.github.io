---
layout: post
title: "Why the _posts Directory is Ignored"
date: 2018-04-03
tags: []
category: jekyll
comments: true
share: true
---

I was recently reading through my "Things I Want to Learn" list and saw a few lines about how Jekyll works and realized that I know the answers to some of those things now!

<!--description-->

I've never understood why the `_posts` directory is listed in the `.gitignore` file. I'd always run `jekyll build` before committing out of some kind of superstition. I knew that directory was being ignored and never pushed, but also had a feeling that there was some kind of building/compilation happening behind the scenes.

Turns out, GitHub Pages does the compilation when you commit! There must be some kind of post-commit hook that executes when you push to remote. I think that's so cool. If I were ever to move to another web host (and pay and get my own domain, which has been on my list for some time now) then I'd have to take that into account. 

To answer another question I asked myself in that post, SASS is, of course, compiled. That makes sense, because you can change a variable and after doing a `jekyll build` regular ol' CSS comes out. It's pretty cool! 

Truthfully, I can't really dig in to the technical nuances of how and why these things happen. But for now, speaking as a hobbyist, I'm still happy with what I've learned anyway.
