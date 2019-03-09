---
layout: post
title: Revolution, Part 1: Audio
date: 2009-06-23 10:00
author: ventspace
comments: true
categories: [Games]
---
A friend of mine has been working very hard on a project which involves two revolutionary technical innovations. I am not usually the kind to care for revolutions, but I honestly believe that both of these are really, really significant. I'm actually hoping to join up and help make these things into a proper product, in fact. These are things you have not seen in games, and he has it working in prototype form on a mobile platform. That's unusual in this industry.

Here, <a href="http://www.youtube.com/watch?v=IUDTlvagjJA&amp;fmt=18">listen to this Youtube video</a> while reading. You need headphones for this, which is why the tech is launching on the iPhone first. The video is not ours, but it is a very good demonstration. Just listen and read. Remember, it doesn't work without headphones -- if you don't have any, it might actually be worth reading this only after you've found some.

The underlying principal is <a href="http://en.wikipedia.org/wiki/Binaural_recording">binaural recording</a>. (This is not the revolutionary bit, and has been around for quite a long time.) Games have had 3D audio for ages, so that is in itself nothing to get excited about. Current 3D audio basically works by modifying channel volumes for playback of a mono sound in order to simulate a 3D space. It works alright if you have a 5.1 setup, but it's not terribly effective in stereo and in general the effect is a bit weak. Binaural recording, however, is a method of recording sounds with a pair of microphones and an actual head model that attempts to produce a stereo sound that simulates what our ears hear. You need headphones because of the recording methodology, and if you're listening to the video I linked, you're probably spazzing out right now.

There is a catch to all this, which is that nobody can synthesize it. The sound is recorded by physically placing it relative to the head, so you can't go back later and place it at an arbitrary location. (Some people have pointed out that there are processors and algorithms that try, but they are expensive and don't really work well.) That's essentially why it's never showed up in games  --although headphones-only isn't a thrilling restriction, either. Still enjoying the barber?

<a href="http://digdagga.com/audio/Ghost Intro2.mp3">Here is his binaural recording</a>. (It's 8 seconds, just pause the barber.) There's one key difference, though. That's <i>not</i> a binaural recording of a sound being moved in front of a recording head. <b>It is done in real-time</b>. (This is the revolutionary bit.) This friend of mine has figured out how to do it. The original implementation worked very well but required a lot of memory and processing power. But the current system is efficient enough to fit on the iPhone. I've seen and heard the demo, working in real time off an iPod Touch. It works well enough to make your skin crawl, like those scissors are probably doing right now if you're still listening to the barber.

I can't really say too much about <i>how</i> he's pulled it off, because we think it's kind of a big deal. You'll see iPhone game releases with the technology later this year, and hopefully by early to mid next year we'll be licensing an actual SDK for whatever platform you might care to use. We're fairly confident this is technology people will want, and hey, it wouldn't hurt to forward this post around the office.
