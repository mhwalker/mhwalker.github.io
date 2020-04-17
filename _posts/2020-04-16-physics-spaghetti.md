---
layout: post
title:  "How I Learned to Love Spaghetti Code (or Why Physicists Fear No System)"
date:   2020-04-16 8:00:00 -0800
tags: physics tech
---
A few days ago, a colleague of mine, Peter Chng, wrote a blog about [The Bus Factor](https://peterchng.com/blog/2020/04/12/the-bus-factor/). Within large physics experiments, it's well known that probably hundreds of operational services have bus factors of 1 or 2 and it's the same for almost every single paper published by them. But large physics experiments also simultaneously have the problem of large fractions of the workforce leaving regularly, which means the situation where nobody understands a piece of code is very common. In this post, I would like to discuss how this situation came to be and why physicists have a special strength as a result: willingness and confidence to look anywhere to understand a problem.

## Why So Many Low Bus Factors

The anti-patterns to virtually every best practice that Peter recommended for avoiding low bus factors are present in large experiments:
* Sharing knowledge is uncommon because operational knowledge is generally not valued.
* Many code bases for both operations and analysis are in private or undocumented repositories (if in version control at all).
* Virtually every expert is a "phone-holder" with their number programmed into on-call phones, meaning it is very easy to hand problems over to experts.
* Deployment procedures may be entirely undocumented, highly manual, and/or rely on the fabled "parallel-scp.sh".
* Almost all code is custom because systems and analyses are unique in the world.
* There is no resource to do work besides the expert, so training a second person isn't an option. Until the bus factor becomes 0.

## All Code is Spaghetti Code

The situation is made worse by the fact that the vast majority of code is produced by graduate students with little or no training in coding, much less software engineering. In my case, the sum total of my formal programming training was two undergraduate courses: Introduction to C and Numerical Methods (taught by physicists), each of which were three credits, the same as a gym class. It's rare for a physics program to have any programming requirements at all.

I will limit myself to only one example from all the crazy things I saw. When I started my postdoc, I inherited some analysis code from a previous postdoc that produced tables and figures for a certain analysis that were needed for papers. All of the code was in a single file of approximately 35,000 lines. It, of course, contained a lot of repeated code blocks, mixtures of configurations and logic, and control sequences handled by commenting/uncommenting. The main method had at least 30 undocumented and poorly named arguments. It took more than a year to unwind this into two libraries that could be understood by other members of the group.

To be fair, many physicists understand and value good engineering. In the large experiments, there are also small corps of bona-fide software engineers working on some of the important core software components.

## No Choice but to Succeed

What can you do when the person who wrote the code is long gone, when they didn't leave any documentation, and the system is totally unique, so that the Internet will surely be devoid of answers? Many physicists have faced this exact situation, and won, having relied on their wits alone to prevail.

In 2013, a long shutdown of the LHC took place so the accelerator and detectors could perform substantial upgrades. One of the CMS upgrades was to the system that controls the clock, which ensures all other systems are in sync down to the nano-second. This change is analogous to a change in API, and because all other systems rely on the clock, all systems had to make a migration. One of the systems reported that they did not have anyone on their team who was familiar enough with the relevant code to make the required change. The system I worked on did not report the same issue, but the person assigned to make the change had also never worked with the code that dealt with the clock. I know because that person was me. Nevertheless, all CMS systems delivered the change on time.

At least half of the members of my group in grad school were forced to debug libraries written in Fortran that underly large parts of the physics simulation ecosystem. I would be surprised to learn that there is any university in the world with a course using Fortran. But I can tell you those bugs are fixed.

Problems that physicists face aren't limited to software. Here are a few root causes to problems that I or my colleagues eventually tracked down:
* A cable was not seated properly.
* Multiple cables were mixed up at one end.
* A system had mixed up the coordinate axes - e.g. (x, y, z) -> (z, x, y).
* A specific bit in a register was flipping every time something was written to it, regardless of the expected value.
* The dew point of a system was not controlled correctly, causing dew to form inadvertently, which dripped into electronics below.
* The power through a metal connection was being driven at its resonant frequency, eventually causing it to snap.

While not ideal for the productivity or maintainability of physics experiments, these situations present a crucible for physicists to hone their investigative skills and their linguistic talents for understanding the software equivalent of a very provincial accent.


## Conclusion
Obviously, it's great when any system operated by your company has a team that's well versed in its code and operation. But there may come a day when you have a problem so pernicious that no team will be able to confirm its origin. One that may be caused by a confluence of factors across multiple systems. On that day you will want someone to track down that bug with the confidence, willingness, and experience to dive into any system. No language too obscure, no device too bespoke, no code too convoluted. For that I put to you my recommendation: call a physicist.

