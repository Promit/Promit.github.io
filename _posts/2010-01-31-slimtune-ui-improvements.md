---
layout: post
title: "SlimTune: UI Improvements"
date: 2010-01-31 10:08
author: ventspace
comments: true
categories: [SlimTune]
---
I've been doing some work on SlimTune quietly, and now it's to the point that I have to decide what the next phase of things should be. There are essentially three major groups of work to tackle: UI improvements, memory profiling, and instrumentation. I'll spend this post explaining what I'm looking at with the UI.

One of the ideas I had with SlimTune was to make the front-end pluggable, so that you or I could develop various views of the data that are neatly customized to focused goals, or provide cool new features. That has worked out fairly well, with the caveat that no one except me is writing visualizers. (That's fine, the database format isn't that stable yet.) However, there turned out to be a number of problems with how the views are actually managed.

Each visualizer is attached to a particular connection, which represents an underlying database and potentially a running application being profiled. That's fine, except that all of the connections and visualizers are jammed into the same window. That's fairly confusing. So step one is going to be to separate each connection into a separate window, and let them all function separately. The transition to a single document, multi-window user interface should be a lot easier to organize and keep track of what's being profiled and where the data is available.

I'm also looking to make the visualizer organization within each window more cohesive. Right now the approach is tab based, and the only way to tell tabs apart is whatever text fits into the caption area. My inspiration for this change comes from an Apple tool called <a href="http://developer.apple.com/mac/library/DOCUMENTATION/DeveloperTools/Conceptual/InstrumentsUserGuide/Introduction/Introduction.html">Instruments</a>. The basic idea is to assign a section of the window to be an overview of all the visualizers that are active, possibly with some type of very simple summary graph. The goal is to improve how much information you can see at-a-glance, and to enable you to get to the information you want more quickly.

I'm also going to be trying to clean up the individual visualizers a little bit, since it seems like they have some rough edges. I'm hoping to integrate at least very basic snapshot support, for example, and repair some general interface problems. The "dotTrace" style will probably get more work done, because honestly I just like that one more. I know some people prefer the NProf view, and I think you're maybe a bit crazy -- but this is exactly why the whole visualization system is pluggable. Everybody doesn't have to work with the exact same UI.

Honestly, how many profilers have you guys worked with where the interface was better than "workable"? My experience has been that the best of the bunch are merely alright, and it goes downhill from there. I don't think SlimTune is great right now, but I feel like the core ideas are there and it's just an organizational problem right now. I'm hoping that with the next release branch, 0.2.x, users will enjoy a much better overall experience.
