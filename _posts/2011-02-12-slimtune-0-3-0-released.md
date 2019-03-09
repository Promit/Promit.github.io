---
layout: post
title: SlimTune 0.3.0 Released
date: 2011-02-12 13:22
author: ventspace
comments: true
categories: [.net, csharp, profiler, SlimTune]
---
Hit the <a href="http://code.google.com/p/slimtune/">SlimTune homepage</a> for the latest release.

The last release of SlimTune was almost exactly a year ago, in February of 2010. The next version has been a very ambitious project, representing a fairly dramatic overhaul of some of the underpinnings. The end result is that, a year later, things look <i>exactly the same</i>. Oops.

In the end, I saw two options. If I did everything I wanted to do, SlimTune 0.3.0 would be an awesome tool, released years too late for anyone to care. Or I could release now, delayed but not catastrophically slow. In terms of the user experience, only two things have changed. .NET 4 applications are now supported. Also, the CLR backend creates its own log file in the user My Documents directory, in order to provide some debugging options in cases that the profiler isn't able to connect.

Some of the things that didn't make it in:
* Native profiling support. The code is "mostly" written, but something about how I'm using DbgHelp and the Sym* functions is badly broken. If someone can help me fix these issues (it's normal C++ code, nothing crazy), I can ship native support. The core sampler pretty much works.
* Silverlight 4. Just a time issue. I'm hoping to do this in the next week or two.
* New visualizers. These were on infinite hold until the core data systems were fixed. They are now fixed, so a lot of UI work is on the way. Again, help would be great. This will be pure C#/WinForms code.
* Snapshots. There's just no UI for them, mostly because the existing visualizers don't handle them correctly at all. Replacing those with NHibernate based visualizers should help substantially.

So...it's out there now. I might make an incremental release with SL4 support, but after that I think it's time to sit down and seriously write a new visualizer that uses the overhauled data backend, supports snapshots, etc. I'm not sure how long that will take. The native profiler? I'll release it as soon as someone fixes it for me.
