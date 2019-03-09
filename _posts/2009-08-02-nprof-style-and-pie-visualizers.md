---
layout: post
title: SlimTune's NProf Style and Pie Visualizers
date: 2009-08-02 19:06
author: ventspace
comments: true
categories: [profiler, SlimTune]
---
<a href="http://slimtune.com">SlimTune</a>'s UI supports pluggable visualizers. What I realized was people were going to want to see their data sliced in different ways, and there would be no sane way to anticipate all those needs, let alone fuse them into a single viewing style. You'll actually be able to drop in .NET assemblies of your own as plugins and have the option to view the profiling data using YOUR plugins, or the ones I ship. Multiple plugins at once on the same data? Check. Real-time view? As long as the plugin supports it. Remote profiling? Absolutely check.

I spent today working on the support for visualizers, and threw together an NProf style visualizer. It's sampling-only, doesn't handle real time yet, and generally a bit rough around the edges. But seeing as how I was typing in SQL by hand before, this is a pretty useful step up. It looks pretty decent, I think:
[caption id="attachment_202" align="alignnone" width="300" caption="NProf-style visualizer"]<a href="http://ventspace.files.wordpress.com/2009/08/080209-nprofstyle.jpg"><img src="http://ventspace.files.wordpress.com/2009/08/080209-nprofstyle.jpg?w=300" alt="NProf-style visualizer" title="080209-NProfStyle" width="300" height="217" class="size-medium wp-image-202" /></a>[/caption]

And then there's this thing that took two or three hours, is a pain in the ass to navigate, but does update in real time:
[caption id="attachment_221" align="alignnone" width="300" caption="Pie chart visualizer :D"]<a href="http://ventspace.files.wordpress.com/2009/08/0802-pies.jpg"><img src="http://ventspace.files.wordpress.com/2009/08/0802-pies.jpg?w=300" alt="Pie chart visualizer :D" title="0802-Pies" width="300" height="207" class="size-medium wp-image-221" /></a>[/caption]
