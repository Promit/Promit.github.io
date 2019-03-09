---
layout: post
title: SlimTune Plans
date: 2009-11-03 22:27
author: ventspace
comments: true
categories: [.net, c++, profiler, SlimTune]
---
I had to sit down and figure out exactly what to do about <a href="http://slimtune.com">SlimTune</a>. People really, really like that little thing, which is amazing for something I put together in six weeks. It doesn't deserve to die, so I'm going to make sure it doesn't. That said, it looks like it will be a while before I get back to it. January, to be exact. Unfortunately, I need to buckle down and finish my degree right now, and I can't really afford to commit to a side project.

That's the bad news. The good news is that I haven't stopped thinking about the project, and I've figured out how to do some really sweet stuff. Let's recap. The current release of SlimTune is a purely sampling based profiler. The goal is to enable instrumentation and hybrid modes for public use. They actually already work, except the data is thrown away by the frontend. I've been unsure of how to collect it in a useful way, but I've got some ideas now that will give you some serious detail. Min/average/max will be there, but who cares? I can give you a <em>graph</em> with timings, showing you the actual characteristics of the function. So if it often takes 5ms or 50ms, but nowhere in between, you'll see two spikes at 5 and 50. The average might be 30ms, but that's basically a bogus number in these situations.

I've also got some UI improvements scheduled. Probably chief among them is to fully enable custom visualizer plugins, so you can throw together your own views of the data. I'd really like to enable some kind of graphing support, but we'll have to see how that goes. It's tricky to find a chart control that also has favorable distribution terms. I was using MS' chart control for the teaser photos, but it really sucks -- or at the very least, I don't understand how to use it the way I want to. Things get sized very strangely, for example. I'll figure something out.

Oh, and people are practically begging for a way to save and load launch configurations. I promise you, that's top of the list. It annoys me too. I did it in six weeks, remember? It will be fixed.

Now, here is the important bit. For those of you who are interested in SlimTune, I have a request! <strong>File issues against the profiler for anything you want.</strong> And I don't just mean bugs. UI and other feature requests are welcome! You can even request things I've promised I will do. That's totally fine. Hit me with everything you've got people! That way I know what's most important to YOU, not me.
