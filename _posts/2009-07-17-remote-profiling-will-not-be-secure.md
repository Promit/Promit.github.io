---
layout: post
title: Remote Profiling will NOT be secure in SlimTune
date: 2009-07-17 16:42
author: ventspace
comments: true
categories: [profiler, SlimTune]
---
At least, not to begin with. There are some drawbacks to not being a security professional, one of which is that I have neither the qualifications nor the experience to do a proper security analysis of the profiler backend. Since I can't audit the backend for security, it will be considered insecure, and that's that.

The practical result of this is that allowing uncontrolled remote connections to the profiler will be incredibly dangerous. I am planning to include a setting that disallows connections except from localhost. However, if you are actually using remote profiling on something that might be attacked, it's critical to make sure it is behind a firewall that will not allow arbitrary connections.

Eventually it should probably allow you to set a username and password for connections, but that's again something that takes some care to implement properly and I'd rather not be the one doing it. In any case, that's functionality which will come much later. Sorry if secure remote profiling is high on your list.
