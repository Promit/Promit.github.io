---
layout: post
title: PhysX and Hardware Acceleration
date: 2010-07-22 14:49
author: ventspace
comments: true
categories: [Ageia, gpu, Novodex, NVIDIA, Physics, PhysX, ppu, Software Engineering]
---
A more accurate title would be PhysX and its <i>lack</i> of hardware acceleration. In retrospect this is largely my fault for believing marketing hype without looking into details, but I figured that I'd discuss it. Finding resources on the subject was difficult, so I think a lot of people may be suffering from the same delusion as I was.

Super short version: PhysX computes its rigid body simulation strictly in software. The GPU never gets involved.

Let's step back for a second and look at the history. It starts with a library called NovodeX which was a popular physics package around the same time Half-Life 2 launched with Havok. A semiconductor company called Ageia acquired the company to build Physics Processing Unit (PPU) based cards, an idea that got a lot of attention at the time. The hardware was over priced, mismanaged, and probably doomed in the end regardless. The effort started sinking pretty quickly until NVIDIA swept in and bought the whole thing. NV then announced that the whole PPU project would be scrapped and that hardware acceleration of physics would be done on the GPU.

The current state of PhysX is somewhat confusing. There are plenty of PhysX accelerated games. Benchmarks of these games show dramatic performance gains when running with hardware supported physics. There have been <a href="http://www.realworldtech.com/page.cfm?ArticleID=RWT070510142143">recent allegations</a> that PhysX's CPU performance is unreasonably poor, possibly to strengthen the case for hardware acceleration. I don't have any comment on that mess, but it's part of the evidence suggesting that hardware is the real focus of PhysX and NVIDIA. That's not a surprise.

What is a surprise is that the rigid body simulation -- the <i>important</i> part of the physics -- is not hardware accelerated. Apparently it was when the PPU rolled out, but the GPU based acceleration does not support it. Look at any PhysX based game that advertises and you'll notice gobs of destruction, cloth, fluids, etc. That's because <a href="http://developer.nvidia.com/object/apex.html">those are the only GPU-accelerated effects PhysX supports</a> (plus a few misc things like soft body). Probably the big tip off is that none of these effects require forces to be imparted back to the main physics scene in any way. This is strictly eye candy.

So the interactive rigid body simulation, the part that actually affects gameplay, is completely in software. And if you believe the claims, it's not even done <i>well</i> in software. All these problems will apparently be fixed in a magical 3.0 release, coming at some vague point in the future. Why? My best guess is that <i>no one has paid any attention to the core PC code in six years</i>. I'd wager that everyone's been so obsessed with hardware acceleration, and that the basic problem of writing a rigid body solver is so stupidly easy, that we're simply coasting on the same 2004 NovodeX era code that made the library popular in the first place. Version 3.0 is probably a ground up rewrite.

Don't get me wrong. PhysX is not <i>bad</i>. It is simply stagnant. Take a recent game and strip away the GPU driven effects candy. What exactly is left in the interactive part of the simulation that wasn't already in Half Life 2? That was also a 2004 release. NVIDIA did what they do best, visual effects. Marketing also did what they do best, letting everybody assume something untrue without actually ever saying it.

Anyway, now you know where hype ends and fact begins. Rigid body game physics is not hardware accelerated; only special effects that fall pretty loosely into the category of "physics" are. Maybe that's common knowledge, but it was news to me.

<b>Update:</b> <a href="https://docs.google.com/viewer?url=http://www.nvidia.com/content/GTC/documents/1077_GTC09.pdf">This presentation</a> about Bullet's GPU acceleration is a good read.</a>
<b>Update 2:</b> I'm wrong on one technical point -- PhysX's hardware accelerated systems <i>can</i> impart forces back to the main scene, and their cloth shows off this capability in the samples.
