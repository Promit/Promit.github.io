---
layout: post
title: SlimTune Profiler for .NET
date: 2009-07-13 15:20
author: ventspace
comments: true
categories: [.net, CLR, csharp, Performance, profiler, SlimTune, Software Engineering]
---
I basically took last week off from blogging. Time to try and get some new entries out! Things have been very SlimDX focused, but what did you expect? It's what I do. Maybe today's will inspire a bit more general interest.
<hr>

As a creature of GameDev.Net, I get to see lots and lots of discussions questioning whether or not C# and .NET are "fast enough" for games. What I don't much is people working on actually analyzing and tuning the performance of their .NET code to see what's going on. I'm not sure why this is, but I have a theory that it's partly because of the sorry state of available performance tools. The only version of VS that has a profiler is Team Edition, which damned near nobody has. Other commercial offerings are also seriously expensive. There are only two free profiling tools that are really available for use: CLR Profiler and NProf. (I've seen a few other tools, but it's clear that they're fringe tools that aren't well supported.)

CLR Profiler is written by Microsoft, and it's a pretty good tool. They've even released the source code, although the licensing is vague. It has a few drawbacks though. First of all, it only does memory profile analysis. It does a very good job of tracking allocations and garbage collections, and the visualizations are very well done too. But that's all you get -- no timings of any kind, let alone a breakdown of where time is being spent. Also, it hasn't been updated since late 2005.

Then there's NProf. Oh dear. The good news is it works, barely. That bad news is that's all the favorable comments I have about it. It does simple sampling based profiling only, and will show you a simple tree based breakdown of time spent. It's not that NProf is useless; I've done lots of good performance tuning with it. But this is literally all it can do, and there's a lot more you want from a profiler. The last release was December 2006, and there's some scattered SVN traffic since then but it's basically dead. Support for x64 is apparently doable if you compile from source. I looked at the source, which is also poorly written. I decided immediately that I could do better than this toy, and now I'm putting my money where my mouth is.

I'm working on a new open source profiler tool right now called the SlimTune Profiler. It will probably release in early September, and the initial feature set is taking direct aim at NProf. The initial version will support sampling and instrumentation profiling for .NET 2.0 and above on local <em>and remote</em> machines. A little later on, you'll be able to profile-enable a long running process at zero performance cost, and then profile it in real time for short periods. Imagine running a production server, and actually connecting with the profiler while it's serving real requests to see what's happening.

On the front-end, data will be collected from the profiling backend and dropped into an embedded relational database. There will be some preset views of the data, but the idea here is that you should be able to apply your own queries to the data and get results that are useful to you. Reporting is not expected for the initial version, but it will be supported eventually as well. I imagine you'll be able to create various tables, graphs, etc and export them, although I'm not sure exactly what format that'll be in. PNG and Excel seem reasonable. I'm hoping that you'll be able to combine results from multiple runs, which would allow you to make all kinds of snappy graphs to show off to your boss.

It's been my plan for some time now to expand beyond SlimDX, and create a suite of Slim software. We've got a good reputation and lots of respect for our work, and I'm looking to build on that. SlimTune is the first step. It probably won't be able to compete with the commercial offerings -- but RedGate ANTS runs $400 or more per license. SlimTune <b>will</b> blow NProf out of the water in a scant two months, and it won't cost you a dime. The feature set is pretty well specified, and the profiler already works in prototype form. The work over the coming weeks is in building a product instead of a project.

And yes, I know I'm a tease. It'll be worth the wait.
