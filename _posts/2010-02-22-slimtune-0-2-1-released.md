---
layout: post
title: SlimTune 0.2.1 Released!
date: 2010-02-22 15:15
author: ventspace
comments: true
categories: [csharp, dotnet, dottrace, nprof, Performance, profiler, profiling, SlimTune]
---
I've been working towards this milestone for some time now, and it's finally ready for public consumption. SlimTune 0.2.1 is out!

<a href="http://code.google.com/p/slimtune/">SlimTune Profiler on Google Code</a>

This version sports a newly retooled interface, along with numerous stability improvements and several new features. To recap, here's some of the cool things SlimTune can do:
<ul>
	<li><strong>Live Profiling</strong> - Why should you have to wait until your program has ended to see results? SlimTune reports results almost immediately, while your code is still running. See your bottlenecks in real-time, not after the fact.</li>
	<li> <strong>Remote Profiling</strong> - Other tools must be run on the same machine as the application being profiled, which can be inconvenient and worse, can interfere with the results. Remote profiling is an integral part of SlimTune.</li>
	<li> <strong>On-Demand Profiling</strong> - Just because your code's running doesn't mean you want the profiler interfering. SlimTune lets you profile exactly when and where you need it, so you can focus on the results you need instead of filtering uninteresting data.</li>
	<li><strong>SQL Database Storage</strong> - Instead of developing a custom, opaque file format, we use well known SQL database formats for our results files. That means you don't have to rely on SlimTune to be able to read your files.</li>
	<li><strong>Multiple Visualizations</strong> - Most performance tools offer a single preset view of your data. Don't like it, or want it sliced differently? Tough. With SlimTune, multiple visualizers ship out of the box to show you what you want to see, the way you want to see it.</li>
	<li><strong>Plugin Support</strong> - We're doing our best to produce the most useful visualizations, but that doesn't mean your needs are the same as everyone else. A few dozen lines of standard SQL and C# code are all it takes to drop in your own view of the performance data, focused on what YOU want to see.</li>
</ul>
And yes, I know the writing is a bit cheesy, but product pages usually are. 

SlimTune was started because the .NET profilers out there suck, or are expensive. I thought there should be a good, open source product out there to support all the developers who are doing real work, but can't necessarily spend hundreds of dollars per seat for licensing. The more people using SlimTune, the better the feedback will be and the better the thing will get. Please help spread the word, and if you're using SlimTune for something cool, let me know!
