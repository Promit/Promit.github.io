---
layout: post
title: SlimTune Profiler 0.1.5 Released!
date: 2009-08-13 15:10
author: ventspace
comments: true
categories: [.net, CLR, csharp, nprof, Performance, profiler, SlimTune]
---
Let's recap. For about two months now, I've been working on a brand new profiling tool for .NET, C#, CLR, and all that jazz. It's open source, completely free, and supports frameworks 2.0 and later (no 1.x, sorry). Some of the notable features include remote profiling, real time results analysis, and multiple visualizers. Today, the first public release, version 0.1.5, is available to the public.

<b><a href="http://code.google.com/p/slimtune/">Project Homepage</a></b>
<b><a href="http://slimtune.googlecode.com/files/SlimTune-0.1.5.exe">Direct Link to Installer</a></b>

Although this is still an early version, it is already quite capable. It supports sampling mode profiling for both x86 and x64 applications, and provides views that will be familiar to users of NProf or dotTrace. Speaking of NProf, it's my belief that this completely replaces it for .NET 2.0+, with a better UI and more features too. (And a far more lenient source code license as well.) There is still a lot to come, of course, but with this release I finally feel that this is ready for the general public.

I'm looking forward to getting lots of feedback, both positive and negative, and I hope that this is a useful tool for everyone.

(P.S. If you want to build from source, you'll need to do it with a non-express version of VC++ 2008 SP1 and VC# 08, with a full boost installation. Also install the SQL Server Compact redist, which is in the repository under trunk\install\ExtraFiles.)
