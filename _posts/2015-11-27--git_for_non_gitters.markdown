---
layout: post
title: "git for statisticians"
cover: cover.jpg
date:   2015-11-27 12:00:00
categories: posts
published: true
---
As a believer in the value of VCS I've recently prepared a talk on Git, specifically aimed at statisticians (my co-workers); in this post I'll go through some of my thoughts and struggles in finding a way to talk to them. 

I'm convinced that modern statisticians require programming skills. Data manipulation, modelling, simulation and visualisation are nowadays hard to conceive without tools like R, Python or Julia. If we include _data science_ (e.g. big data and machine learning) then there's no doubt about it, as it just wouldn't exist without computers.

Many statisticians have already figured that out but, as far as I can tell, many haven't yet taken advantadge of a great deal of the knowledge, tools and practices that programmers have been gathering for a long time. Among all those tools and practices, the use of version control is something that I would prioritise.

Version Control Systems (VCS) have been around for almost 50 years, however, it's a step that most statisticians I've come across are taking on very slowly. They find it daunting and have trouble understanding how the effort of learning such a tool could ever pay back. In my experience, explaining Git to lay persons has been rather difficult and often not very gratifying.

[xkdc] (http://xkdc.net/1576) exemplifies with clarity what I want to avoid and I'm quite sure is extremely frequent.

![imatge](/images/git.png)

My main challenge was to correctly identify the key aspects that would trigger the audience's interest. The audience will mostly be composed by statisticians, I mean old school statisticians not data scientists. They write code in SQL, SAS and/or VBA, and a few might know some R. Most of them have never used a shell command line, definitely not in a \*nix operative system. They are certainly convinced that using the command line is something from the last century. In a nutshell, they are _not_ programmers and not particularly computer savvy.

In my research, I found very little literature specifically focusing on how to approach statisticians. These are two of the few examples that are clearly targeting this kind of audience.

[A statistician's initial experiences of Git and GitHub - by Jonathan Bartlet](http://thestatsgeek.com/2015/05/16/a-statisticians-initial-experiences-of-gitgithub/)

[Git and GitHub - by Hadley Wickham](http://r-pkgs.had.co.nz/git.html)

[RStudio](https://www.rstudio.com/) includes features to [handle versions](https://www.rstudio.com/resources/webinars/collaboration-and-time-travel-version-control-with-git-github-and-rstudio/). I much prefer to handle versions outside RStudio but to be fair they're one of the few that are making an effort to facilitate the process to statisticians.

There are plenty of sources focusing on programmers, which are useful but, miss the point a bit.

Here some of  my considerations on specific aspects of approaching the subject:

1. It's an improvement to Subversion! It's distributed! Well, most statisticians I've come across don't care. They've never used a VCS before, so talking about Subversion or any other VCS is only relevant to them in terms of historical context. The whole fuss about the VCS being distributed only adds noise to their understanding. They don't really care about centralised VCS, why should they!? They want to learn whether that thing I'm showing them is useful on its own not in relation to something they also don't understand. I'm convinced they need to understand how a distributed version control system works, but not in relation to other approaches - if they care, they sure will look at it later. As a consequence, in my talk I've reduced the historical context to a minimum.

2. My code is short and simple, therefore I might not need version control at all. 
Statisticians frequently need very short scripts to run specific processes, but the level of complexity of the code that justifies using a VCS is usually not addressed in any material I've seen.
 As I said, they come from not using any VCS, in some occasions only they feel they would have benefited by some sort of better versions organisation. Learning a new tool is tough, therefore their first questions are: Do I really need this? Is a VCS only necessary for a big collaborative piece of work? Do a couple of scripts to build a regression model require the extra work of committing every single change? I don't really have an answer to that. I personally put any project under Git because it helps me be a bit more structured in what I do and helps me coming back to it in the future. Also I find it's a matter of habitude, if you're used to use it for small projects you will use it when the need comes for larger projects. I probably wouldn't need it for each and every project, but it's easier to have a single way of working than having to create a specific one for every little project I start.

3. What am I tracking? It is a very basic doubt, but it can be confusing. These people are used at looking at files not at projects, therefore it takes some time to convey the fact that we're not tracking each single file separately but a whole directory at once.

4. Flexibility of workflows is great, but, how do I start? In my own experience the first step of setting up a team to work with Git was difficult in deciding the workflow. Even if it's a very important element in a team, and once it's established it's very simple, it can be a roadblock to start. A new person that is struggling with getting the jargon and the new tool features, appreciates a suggestion of a workflow to start with. They will have time to change it to something that fully adapts to their needs.



