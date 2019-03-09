---
layout: post
title: SlimDX Compile Times
date: 2009-06-11 12:53
author: ventspace
comments: true
categories: [SlimDX]
---
My new sawzall is awesome, though I've managed to more or less wreck the wood cutting blade that comes with it.
<hr>

When I was doing the November 08 release of SlimDX, I had to do a <em>lot</em> of builds to get it right. As a result, I was keenly aware of compile times, which sucked to be quite frank. On my main development workstation, a full rebuild of Release x86 was around ten minutes. Debug was a bit quicker, but still around eight minutes. I don't have a very powerful machine at home, but when I tested the fairly powerful system at work, it still didn't go terribly fast. It turns out that you really don't have to accept the long builds that naive project layout creates.

I've just now done a rebuild of Release x86 in under five minutes. A few relatively simple steps have cut it in half; Debug builds are almost down to a third. The changes, although tedious, were relatively simple and mechanical, and didn't really involve any increased risks for bugs. That's a very good thing with large changes, especially when trying to attack problems that are arguably not THAT important.

The first change was to roll out precompiled headers across the library. Our PCH file is extremely simple; it just includes all of the system and DirectX headers, and a handful of library headers. These headers are fairly massive though, and we saw benefits from the PCH basically immediately. I'd avoided PCH at the beginning because I've always found it to be a pain in the ass, but the trade off was worth it. It helps that the headers in there don't really change.

The other trick was a bit more subtle. See, Visual C++ compiles all of its input .cpp files to .obj files in the project intermediate directory. However, it does not preserve any input directory structure into that folder. So in SlimDX, we had direct3d9/Device.cpp, direct3d10/Device.cpp, directinput/Device.cpp, and so on. In the intermediate directory, these became Device1.obj, Device2.obj, Device3.obj, etc. That's not compiler-automatic, either -- it has to be configured in the project build settings per file. The IDE is capable of doing that step automatically, but when it comes to actually doing the build, the compiler process has to be spun up separately for each individual file. Compared to the amount of compilation required for any given C++ file, this is a fairly substantial cost, especially when multiplied by a hundred or so.

Once we renamed the files so they no longer conflicted in a flat directory structure, we got the current build timings. In fact, it takes almost as much time to link as it does to compile. (I'm sad that linking is still a single threaded operation.) Not bad, all in all.
