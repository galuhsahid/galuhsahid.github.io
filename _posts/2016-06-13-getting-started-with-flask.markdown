---
layout: post
title:  "Getting Started with Flask"
date:   2016-06-13 19:12:17 +0700
tag: python
blog: true
---
At this moment we're currently opening registrations for CompFest 8's [Competition][competition] and [Academy][academy]. I thought it would be nice to have a glimpse of how we're doing right now. We have a dashboard listing all applicants, but we didn't have enough time to build a fancy dashboard showing graphs--and boy do I love graphs. Plenty of meetings and discussions later, it is clear that numbers are important. However, sometimes speaking with numbers is not enough. Graphs do really put things into perspective. So, I thought I'd make a quick Python script (because seaborn, duh).

The datasets are interesting and I'm already thinking of some things to analyze, though I'd wait until the registration closes (and maybe until CompFest is over, ha) to deal with all the fun stuff. Anyway, eventually I got the script I wanted up and running, though its job is pretty simple: tell us how many registrants away we're from our targets, our current registrant rate, and give us a pretty graph to look at. Simple.

I want everyone to be able to use it from the comfort of their home without having the script, so I thought again, hey let's put up a web application. The thing is I didn't know how to do it with Python, but I wanted to do it with Python. A couple of web searches later I stumbled upon [Flask][flask], a micro web framework written in Python. Sweet. I just want to get something simple up and running fast. Django would be too much, and it seems like Flask is just right. It's a quick hack so it's rather messy but [it's on my GitHub anyway][cf-stats].

[competition]: http://compfest.web.id/competition
[academy]:   https://compfest.web.id/academy
[flask]: http://flask.pocoo.org
[cf-stats]: https://github.com/galuhsahid/cf-stats 