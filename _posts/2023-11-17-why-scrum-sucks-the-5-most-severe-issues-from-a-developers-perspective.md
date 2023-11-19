---
title: "Why Scrum sucks - The 5 most severe issues from a developer's perspective"
layout: post
---

***SCRUM SUCKS!*** Many people have written their version of this post and here is mine. 

In this post I describe what I consider to be the main problems with Scrum found in the wild. I experienced all of them first-hand when working in multiple Scrum environments. I also know for a fact these are the same problems developers in all kinds of places experience as well. Each section also includes a suggestion how to fix the problem.


In this post I concentrate on the first set of the five most harmful malpractices in corporate Scrum: 
- The daily is a status meeting
- Treating story points as time
- Treating estimates as deadlines
- Attempting to plan multiple sprints ahead
- Unclear acceptance criteria

Read this second post for my thoughts on the second set of also not ideal, but less severe, problems.


### "Not TRUE Scrum...!"

First off, I know there's the frequent argument that allegedly "Scrum sucks" only when it isn't implemented the right way. If you're one of them, let me rephrase this - what corporations call "Agile" and "Scrum", the way it is implemented in 99.9% of places the in real world and what is experienced by 99.9% of developers sucks. In practical terms the difference doesn't matter. Either way - no matter if Scrum itself sucks of basically everyone is too incompetent to do it right - companies attempting to do Scrum end up with dysfunctional practices. Also check this page about the [No true Scotsman](https://en.wikipedia.org/wiki/No_true_Scotsman) fallacy.

### The Daily is a Status Meeting

The daily Scrum, or standup, being treated as a status report meeting is one of the most dreadful aspects of being a software developer in 2023.

The root cause of this is non-developers participating in these meetings. This leads to all kinds of frustrations and unintended behavior but luckily that's one of the few cases where there's an obvious and simple to implement fix.

**Fix:** Non-developers being present in this meeting is extremely harmful, no matter their intentions.

Because this is such an important topic, I wrote separate post explaining why non-developers in these meetings are such a huge issue and which harmful consequences this causes.


### Treating Story Points as Time

Story points (or T-shirt sizes or whatever) are meant to estimate complexity (not time). It's a common malpractice to multiply them by x to get an estimate of developer hours needed to implement the story. In software development, estimates are nearly impossible to get right. That's because unforeseen complexities are only regularly only discovered during the implementation phase. To protect the development team from being expected to complete an unknown amount of work in a fixed amount of time these obfuscations were proposed. Treating these story points as time anyway does the exact opposite and renders the idea in itself pointless. When you're treating story points as time and expect the team to complete a certain number of story points per time, you're setting up the team (and thus your project) for failure. Don't do this!

Ron Jeffries, the one who came up with the idea of story points, is aware of this common malpractice and even [apologized for coming up with the idea](https://ronjeffries.com/articles/019-01ff/story-points/Index.html). He also concluded that [estimation in itself is "evil"](https://ronjeffries.com/articles/021-01ff/estimation-is-evil/).

**Fix:** The quick solution is simply stop treating story points as time. They aren't. And you doing so destroys the trust between you and the development team. For more thoughts on the topic read this other post by me to learn what I think is a better approach to estimation in software engineering.


### Treating Estimates as Deadlines

Corporate Scrum often suffers from the misconception that estimates are rigid deadlines. "Last sprint you completed 30 story points but this time it's only 12. Clearly the developers must be slacking off and need to be whipped more" might be the line of thinking of those who have never worked as a software engineer.

This misconception fuels unnecessary stress and compromises the quality of work. Developers rush to call something finished that they know isn't actually finished just to abide by the arbitrary sprint deadline. They are fully aware this will resurface as a bug later and that this will ultumately cause more work than needed but treating estimates as deadlines (and story points as time, see above) incenticives them to act this way.

A shift in mindset is needed, emphasizing that estimates are approximations meant to guide planning, not immutable deadlines. The actual thing you should be concerned with is working software being produced. Treating estimates that are nearly impossible to get right as deadlines does the opposite. It incentivises people to lie to you and pretend that half-finished work is actually finished. That's how you steer the project towards becoming a mess of a codebase.

You might cause even more damage than you're aware of. This practice is taxing on the mental health of developers to say the least. Imagine at all times being on a deadline that's at most (!) two weeks away and half the time less than a week away. IMHO these arbitrary sprint deadlines are one of the main drivers of burnout in developers and developers quitting a job.

**Fix:** Don't assign blame to missed estimates. If at all possible, do away with estimates altogether. They're not helping the team to develop working software. They're causing unnecassary stress which will cause developers to seek greener pastures which will slow down your project even more.

### Attempting to plan multiple sprints ahead

Scrum is meant to enable agile software development, i.e. being able to quickly react to changing requirements and unforeseen complexities. The idea is that changing the plan every sprint (usually two weeks) will help with working on the highest priority tasks at all time. 

It's almost certain that you will need to update your plan at the end of each sprint. With this in mind, it doesn't make much sense to plan multiple sprints ahead, because they are interdependent. By forcing people to constantly update plans that are doomed from the beginning you create unnecessary busywork. It's an illusion that you can plan months ahead when constantly changing requirements and problems being discovered along the road are the norm. 

Certainly this doesn't help with producing working software, which is what you should actually be concerned with. If you find yourself in a situation where you spend an awful amount of time on producing these nearly worthless estimates, you know you've become the victim of bad management. Even worse, when management wants to hold you accountable for these *estimates* and treats them as deadlines, see the point above.

**Fix:** Convince your higher ups that creating these predictions doesn't add value to the project and often causes unnecessary drama which slows done the development effort. 

### Unclear acceptance criteria

Unclear acceptance criteria often result in scope screep, i.e. work balooning and previously simple tasks becoming more and more complex. 

Picture this, you pick a story that says a certain data source should be loaded into the datalake. Being a mindful developer, you ask the PO what this means *exactly*. Load the raw data into the landing zone only? How frequently? The entire database or just parts of it? Should the data also be analysed? Should the data also be cleansed? Should data quality issues be documented? The answer to all of these questions might turn out to be yes, because all of these are best practices of your team for integrating data sources. As you keep working on these points you discover that assumptions about the data and the source system were invalid and solving these issues creates even more work needed to finish the task as previously defined. This is scope screep. 

Before starting to work on a story there should be clearly defined acceptance criteria and the work should comfortably fit into a sprint with a big enough time buffer to account for unforeseen challenges. In this scenario the story might require you to load table x at interval y into the landing zone only. Everything else will be part of another story with more clearly defined acceptance criteria. The next story might be consulting a business expert on the meaning of the data and adding data quality checks before loading the data into the next zone of your lakehouse architecture. Discovered that assumptions are broken and the data doesn't pass the data quality tests? Well, no problem, acceptance criteria were met. That's an issue for the next sprint, etc.

Having we--defined acceptance criteria in place before the you start your work should be the most basic and will do wonders for your mental health as a developer. Imagine you were signing a contract that you will get this work done by the end of two weeks. You would probably not just sign anything before you read it and you would want your contract partner to spell out exactly what he wants before he's paying you rather than sueing you.

**Fix:** Developers should enforce the PO to write [SMART](https://en.wikipedia.org/wiki/SMART_criteria) accepratance criteria. Perhaps the Definition of Done, a set of requirements that typically includes the acceptance criteria, also needs clarification.

### Closing words

I'm fully aware that the root cause of most of these issues is simply bad management and that bad management is often the result of bad management higher up the hierarchy. In my cases, particularly in big organizations, the root cause is so far removed from the issues that they don't care to fix them. Often you have to choose between doing what's best for the project and what would make the world a little bit better on the one side and what's best for you personally as someone who wants to make a living on the other side.

I still hope you either gained some insight into the reasons why nowadays Scrum is disliked by most developers and almost synonymous to bad management.

There's even more wrong with Scrum from a developer's perspective though. Read this second post for my thoughts on a second set of flaws with Scrum, albeit less severe ones.