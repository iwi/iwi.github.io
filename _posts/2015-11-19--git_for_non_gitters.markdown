---
layout: post
title: "git for statisticians"
cover: cover.jpg
date:   2015-11-19 12:00:00
categories: posts
published: true
---

It's a fact that modern statisticians require programming skills. Data manipulation, modelling, simulation and visualisation are nowadays hard to concieve without tools like R, Python, SAS or Julia. Most statisticians haven't taken advantadge of many of the tools and knowledge that programmers have been gathering and improving for a long time.
Version Control Systems (VCS) have been around for almost 50 years, however, they're a step that the new comers to the programming world are taking on very slowly, most find it dounting and have trouble understanding the necessity. As a believer on VCS I've recently prepared a talk on Git specifically aimed at statisticians; here I'll point out some of the issues I identified while building it up and how I've tryed to address them.

In my experience, explaining Git to lay persons has been rather difficult and often not very gratifying.

[xkdc] (http://xkdc.net/1576) exemplifies with clarity what I want to avoid and I'm sure is extremely frequent.

![imatge](/images/git.png)

My first and main challenge was to correctly identify the key aspects that would trigger the audience's interest. My audience will mostly be composed by statisticians, I mean old school statisticians (some of them not doing anything beyond basic descriptive statistics anymore). Their programming expertise beyond writing scripts to get some basic result out of a table or database is limited. They write code in SQL, SAS, R and/or VBA. They don't know some of the basic concepts of how a software application works (e.g. they sometimes confuse the code with the interpreter/compiler, they confuse the IDE with the engine). Most of them have never used the shell console, definitely not in a \*nix system, and are convinced that point and click is most of the times better. In a nutshell, they are _not_ programmers and not particularly computer savvy.

In my research, I found very little literature specifically focusing on how to approach statisticians. These are two of the few examples that are clearly targetting statisticians.

[A statistician's initial experiences of Git and GitHub - Jonathan Bartlet](http://thestatsgeek.com/2015/05/16/a-statisticians-initial-experiences-of-gitgithub/)

[Git and GitHub - Hadley Wickham](http://r-pkgs.had.co.nz/git.html)

There are plenty of sources focusing on programmers, which are useful but, miss the point a bit. Two of the most recurrent issues:

1. It's an improvement to Subversion! It's distributed! Well, most statisticians I've come across don't care. They've never used a VCS before, so talking about Subversion or any other VCS is only relevant to them in terms of historical context. The whole fuss about the VCS being distributed only adds noise to their understanding. They don't really care about centralised VCS, why should they!? They want to learn whether that thing I'm showing them is useful to them on its own not in relation to something they also don't understand.

2. It's usually not addressed is the level of complexity of code that justifies using a VCS. They come from not using any and only in some occasions feeling they're lost and would have benefitted by some sort of better versions organisation. Is a VCS only necessary for a big collaborative piece of work? Do a couple of scripts to build a regression model require the extra work of committing every single change? I don't really have an answer to that. I personally put any project under Git because it helps me being a bit more structured in what I do and helps me coming back to it in the future. The truth is that I probably wouldn't need it for each and every project, but it's easier to have a single way of working than having to create a specific one for every little project I start.

_Note: I'll add a link to the presentation as soon as it's final_
