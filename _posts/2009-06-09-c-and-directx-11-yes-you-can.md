---
layout: post
title: C# and DirectX 11 -- Yes you can!
date: 2009-06-09 23:29
author: ventspace
comments: true
categories: [SlimDX, SlimDX C# Direct3D DirectX]
---
Along with Windows 7, several beta APIs to go along with the new OS have been available. One of the biggest is Direct3D 11, and although we were dabbling with it before, what was previously available was strictly experimental. Although it is by no means stable, <strong><a href="http://slimdx.org">SlimDX</a> now enjoys full support for Direct3D 11!</strong> Thanks to Mike Popoloski for doing an insane amount of work, again.

Right now, the proper support is only in the Subversion repository. However, our June release should be out shortly and that will have a Windows 7 companion DLL with the D3D 11 support. Remember that it's an SDK-only component, and the runtime won't install it. Direct2D and DirectWrite are in there as well. Now keep in mind that all of these APIs are beta, and our wrappers are unstable as a result too. You'll also have to translate the C++ samples in the DX SDK for the time being, as we don't have our own samples yet. We're working on that, and hopefully we'll have a few samples out later in the year.

There's a lot of cool new stuff in 11. First off, it supports feature levels, just like 10.1. Although you'll have to have Vista SP2+ to run 11 based apps, they will run on any WDDM-compatible hardware, which should include the entire gamut of Direct3D 9 and 10 hardware from all major manufacturers (and I don't just mean the big three). It also has full multithreaded rendering support -- not the faked granular locking that 9 and 10's multithreaded devices have, but proper synchronization. This is supported even on the lower feature levels, so if you can stand to give up XP and pre-SP2 Vista as targets, you might be in line for some big performance benefits. I found <a href="http://www.rorydriscoll.com/2009/04/21/direct3d-11-multithreading/">a really nice blog post</a> that goes into a lot more detail.

There are a number of new shader types, but basically the entire API has been divided into separate <em>draw</em> and <em>dispatch</em> pipelines. The heart of the dispatch pipeline is the <em>compute shader</em>, which serves the same purpose as OpenCL, CUDA, and so on. In other words, it provides support for general purpose GPU (GPGPU) computing. This is a big deal for high performance computing on the Windows platform, since CUDA is NVIDIA-specific and OpenCL support is mythical at best. I'm eager to see some high performance applications which are powered by managed code and then go straight to the GPU for real heavy lifting. (This will support DX 10 class hardware by final release, but support is not available yet.)

SlimDX has got you covered on all of this. These APIs are expected to be final with the launch of Windows 7, which means that our fourth quarter release will have full, non-beta support for 11. (No more separate DLL at that point.) We'd like our implementation to be as good as it can be, and for that, we need users to see how it works out and to let us know about anything we're missing! Sorry Microsoft, but when it comes to DirectX on the .NET platform, we have every intention of being the de facto standard. And remember to check out <a href="http://slimdx.org/features.php">the full features list for SlimDX</a>!
