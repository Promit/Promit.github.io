---
layout: post
title: SlimTune and Services
date: 2010-02-03 12:26
author: ventspace
comments: true
categories: [aspnet, profiling, services, SlimTune]
---
I've now added service and ASP.NET profiling support to SlimTune. Services seem to work, although I've tested it pretty minimally. ASP.NET <i>should</i> work but I'm really not set up to test it properly. I might attempt a VM based test later, because I'm not really sure what sort of status IIS is on my main machine and I don't really want to find out. Still, ASP.NET is little more than a special case of the service profiling.

Unfortunately, it turns out that admin level privileges are required in order to control (and therefore profile) services. I've decided not to automatically request privilege escalation in SlimTune. Instead, it simply blocks you from selecting service or ASP.NET profiling unless you're running with administrator privileges (and pops up a message box explaining this). You'll have to right click -&gt; Run as Administrator in order to select those options, unless you've turned off UAC outright. Kind of annoying, but I figured it was the best compromise.

Incidentally, the code to support this is mostly cloned out of CLRProfiler, with some work to integrate it into SlimTune smoothly. I don't even understand why half of it is there. You know why? It's because <a href="http://social.msdn.microsoft.com/Forums/en/netfxtoolsdev/thread/40e33ee5-337d-4d99-be87-dd323c154253">there's no other documentation</a> about how to do it. This problem is somewhat endemic across all the .NET profiling bits, unfortunately. It may also affect the debugger documentation, but I'm not sure.

SlimTune is essentially patched together using a combination of <A href="http://www.codeproject.com/KB/dotnet/dotnetprofiler.aspx">a helpful CodeProject sample</a>, the official documentation, blog posts, MSDN forum questions, and experimentation. I'm hopefully that by publishing a reasonably full featured open source profiler, I'm also creating a good practical reference on how to handle the guts of .NET. I've considered blogging more about the internals as well, but that remains to be seen.

[Update] I would like to mention, though, that this is vastly more documentation than is available for Mono.
