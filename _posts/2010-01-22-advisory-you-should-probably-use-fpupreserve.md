---
layout: post
title: "Advisory: You Should Probably Use FpuPreserve"
date: 2010-01-22 13:37
author: ventspace
comments: true
categories: [SlimDX]
---
One of the create flags for D3D 9 devices is FpuPreserve. It tells the Direct3D runtime that you don't want it to mess with the FPU flags, which it does to improve performance. And you should probably be using it.

FPU computations are a really hideously messy area, one that makes my head spin <a href="http://msdn.microsoft.com/en-us/library/aa289157%28VS.71%29.aspx">when you get into the details</a>. One of the cool things .NET does is to take away most of the complexity and make very simple, straightforward guarantees about how floating point code must behave. Operations are done at specifically defined precision, in a certain way, and the runtime must enforce these requirements during code generation. (Which creates some weird x86 code.)

When DirectX goes in and messes with FPU state, it apparently throws off what .NET expects, leading to <a href="http://stackoverflow.com/questions/1060158/mdx-slimdx-messes-up-wpf-scrollbars">weird bugs</a> <a href="http://stackoverflow.com/questions/1967266/stopwatch-returns-integer-after-directx-device-creation">of various sorts</a>. So unless you've got some problem with FPU performance that can be solved by switching to the faster FPU states (hint: you don't), it's probably a good idea to simply set FpuPreserve all the time.

[EDIT] I forgot to mention as an addendum that as part of the SlimDX 2.0 transition, it's very likely this flag will become the default.
