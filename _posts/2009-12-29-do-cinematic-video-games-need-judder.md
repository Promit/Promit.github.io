---
layout: post
title: Do cinematic video games need judder?
date: 2009-12-29 23:38
author: ventspace
comments: true
categories: [Games, Graphics, judder]
---
Just a thought. I've been learning a lot about television technology lately, and one of the tricky things about it is the difference between video and film. It's generally well known that movies and film are at 24 fps, supposedly because that's the frame rate at which we can't distinguish it from real motion. (That's bullshit by the way, and has nearly nothing to do with why film is at 24 fps.) Video, on the other hand, is run at 25/50 fps (PAL interlaced or progressive) or 30/60 fps (NTSC interlaced or progressive). This means that video has a very distinctly different look from film, and film never ends up looking quite right on normal televisions. The introduction of 120hz LCD TVs on the market is partly intended to combat this problem, and show film sources at their true frame rate.

However, as far as I can tell (and I haven't looked very hard), no one in video games has thought to apply this in reverse. Film people have, and movies like Avatar have film grain and film judder (non-smooth motion) applied in post production. We've been using film grain post filters in video games for a little while now; Mass Effect and WET come to mind. I'm pretty sure they're still running at 30/60 fps though, which isn't right if you want to look like a movie. It's half-hearted.

Now even though the consoles can configure their hardware to output 1080p/24 (1080 vertical progressive scan lines at 24 fps), I don't know if it's available (or permissible!) to run at that setting. So the thing to do if we want video games to look like movies is to render internally at 24 fps, and perform <a href="http://en.wikipedia.org/wiki/Telecine#2:3_pulldown">2:3 pulldown telecine</a> live. The altered cadence will provide the proper film feel. (Ironically, most higher end televisions now have dedicated hardware to counteract this effect and restore the stream that we started with.) That will let us approximate the desired 24 fps, while still running at 60 fps.

Those of you who are paying attention will have realized that this involves repeating frames at an uneven rate that is <i>not interpolated</i> to the current physics state. Won't it introduce jerkiness when things are moving around? Why yes, yes it will. That's the whole point.
