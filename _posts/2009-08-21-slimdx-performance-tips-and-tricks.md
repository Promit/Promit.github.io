---
layout: post
title: SlimDX Performance Tips and Tricks
date: 2009-08-21 11:31
author: ventspace
comments: true
categories: [Performance, SlimDX]
---
Previously, I discussed some of the inherent performance costs that SlimDX suffers. Although that's somewhat educational if you're evaluating SlimDX, it's pretty useless if you're already using it and would like to get the most out of your code. This time, I'll go over what you can actually do to make sure you're running optimally.

There are two big problems with managed vector/matrix math, and this applies just as well to XNA. First, all of the types are value types, and passed by value to operators. That means when you multiply two matrices via operator*, two matrices (32 floats, 128 bytes) are copied onto the stack, and then another one is copied back into your result. This can get quite expensive, and the solution is to pass by reference, not by value. Unfortunately that means operators are a problem for performance sensitive code; you'll have to use functions like Add and Multiply instead.

There's also the problem that generally speaking, vector operations are not candidates for inlining. They're too big for the JIT's metrics to pick them up as inlining candidates (the 3.5 SP1 revision may have changed this). For small vector operations, this can again become a substantial cost. Unfortunately this is a messy one to deal with, as you can't ask the compiler or JIT to inline things for you. The most effective approach I've seen is to replace vector operations in stable code with hand-inlined code. Farseer Physics uses this method, and wraps the inlined blocks in #region to clarify what's going on. Yes it's incredibly tedious, but if that's what you have to do, then there it is.

Don't use strings as effect handles if you can help it. We have to convert from Unicode to ANSI internally, and create a temporary handle. This gets slow and can cause other bugs as well. In future releases, this problem will actually be alleviated somewhat, but it's best to avoid it completely.

Also make sure that SlimDX itself is configured correctly. These settings in the Configuration class. For example, object tracking is an incredibly useful debugging feature that tells you what objects you're leaking and where they came from. But because it records call stacks, it's also quite expensive. The default setting is for it to be active; turn it off for production builds. Also consider disabling exceptions for return codes you don't care about (device lost and device not reset are common ones), instead of catching and ignoring.

Be careful with get properties and functions. An object's Description property will always call GetDesc() on the underlying object, and then return a whole struct. This can get expensive quickly, especially if you casually access the property multiple times. We've chosen not to cache much of anything in SlimDX for the time being due to some nasty bugs early on. Querying data is expensive as a result.

Anything involving callbacks and callback interfaces is bad news, and it'd be best to avoid them for performance critical code. Every time you cross the boundary from managed to unmanaged or back again involves overhead, and for callbacks we end up bouncing multiple times -- all while doing various kinds of fix up and data marshaling. Texture.Fill in particular is incredibly slow.

If you're working with large amounts of raw data that will be sent to SlimDX, consider using DataStream, especially as a replacement for (Unmanaged)MemoryStream. When you give SlimDX a generic Stream to work with, it has to allocate a buffer large enough to hold the data, read all the data into the buffer, and then copy that into the target native DirectX buffer. This is quite inefficient for certain types of data that are already in memory. If you hand us a DataStream, we can skip the allocation and read, doing a fast memory copy only.

Hopefully that's helpful. I'll update this post as I remember more tips.
