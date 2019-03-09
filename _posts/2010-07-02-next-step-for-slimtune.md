---
layout: post
title: Next Step for SlimTune
date: 2010-07-02 20:07
author: ventspace
comments: true
categories: [.net, csharp, profiler, SlimTune]
---
Okay, it's been a long time since I touched Tune. In fact I think I'm averaging one major blast of work just about every six months. That's <i>terrible</i>, but I don't really know what to do about it. I don't have the bandwidth to run a company and two open source projects at once. Even my involvement with SlimDX is much weaker than it used to be. The difference is that DX has more very competent developers to take care of it; Tune is all on me.

All the same I <i>am</i> back and I'm working on improving the thing once again. It's been long enough that I can't remember exactly what the roadmap was or what I was trying to accomplish. (Protip: Write down your roadmaps, people!) That gave me an opportunity to step back and really examine what the state of Tune is. A lot of people were pulling for memory profiling support, which I had slated for 0.3.x and planned to start fairly soon. I no longer think that's a good idea. It's more in my (and hopefully your) interests to make the 0.2.x series as strong as possible.

The basic problem is that SlimTune is a really, really cool program with a very mediocre implementation. I still think the ideas in there blow away a lot of the commercial profiling tools out there. All the same, I'm one person and the code's accumulated about eight weeks of full time work since its beginning (8*40 = 320 total man hours). It's still a prototype. Before I can do any significant expansion of features, I first need to rebuild the foundation in a stable fashion. The database code is basically a complete loss. The UI is a rough draft. The backend is actually pretty solid, as far as I know, but it isn't good with error reporting. Things just stop working sometimes.

This is stuff that needs to be tackled long before adding more features. That means I'm not going to see very many users soon, and that corporate adoption will be slow at best. I have several good friends on commercial products who need memory tracking, end of story. But honestly, I have a reputation of building quality work and it's not casually earned.

In short, the next phase of development is to build the best damn sampling profiler out there.
