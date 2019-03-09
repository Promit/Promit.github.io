---
layout: post
title: Finalizers and DirectX 11
date: 2009-06-22 10:00
author: ventspace
comments: true
categories: [Direct3D, Direct3D 11, DirectX, DirectX 11, SlimDX]
---
Long story short: I'm not so sure about them.

The reason SlimDX does not, by and large, include finalizers is because of the threading issues involved. You can enable multithreaded devices in D3D 9 and 10, but it's not really a good idea in most cases and we don't see any reason to handle it differently. Direct3D 11, however, is making a big push for multithreaded rendering, and that means we could credibly enable finalizers just for it. There is a single threaded flag, but we can detect that and avoid finalizing those objects. But there's more to it than that.

Let's recap. Finalizers generally only make sense as part of the IDisposable pattern. The idea is that you expose Dispose to allow deterministic destruction of unmanaged objects, but in the case that you don't call Dispose -- deliberately or accidentally -- the finalizer comes around and does it for you. This is mostly designed for Windows handles and the like, which I think is why it fails to really take threading into account. Because most DirectX objects are <em>not</em> free threaded, that goes to hell in SlimDX. We chose to eschew the finalization part of the pattern completely, and nobody really seems to mind.

From a strictly technical standpoint, enabling finalizers for 11 would be easy. It would take less time than writing this, actually. Josh pointed out that it's a break in consistency, but I believe we can at least make an argument for it being part of the multithreaded support. The Windows API Code Pack provides finalizers, which tends to support the idea we should do it. However, there are a number of issues I'm concerned about, and for June we almost certainly won't have them. I'm not sure we ever will.

At some level, finalizers exist to patch up programmer error. While this is not necessarily a bad thing, I have some misgivings about it. SlimDX doesn't clean up after you, but it does provide some very powerful facilities for finding out what didn't get cleaned up, and we even have some options for expanding that functionality. Finalizers will tend to interfere with that tracking in unpredictable (but innocuous) ways. If you have repeating leaks, maybe some will get finalized and maybe some won't. The ObjectTable won't be able to provide you with reliable results. We could provide a configuration option for it, like we do for exceptions -- but that's not a good idea. Basically if you're exposing a choice to the user, it means you didn't have the balls to actually make the choice and you're passing the buck. I think Raymond Chen said something similar at one point, in not quite the same language.

There's also a question of interop objects. We can track the single threaded flag in SlimDX code, but not in externally created objects. Sure it might be possible to get the device for an object and check its caps (I haven't checked), but even if that works sometimes, it's extra overhead and I'm guessing that the number of special cases involved is prohibitive. We could assume all external objects are multithreaded, and then someone will come complaining about a decision that we can no longer reverse, and which is impacting their performance negatively. Supporting interop smoothly is a critical use case for us, not least because it's a major differentiating point against XNA. It has worked very well so far and I'm not thrilled about potentially breaking it now.

I guess I was lying at the beginning, actually. I am fairly sure about them. In fact, I'm fairly sure we won't have them. They are looking to cause more problems than they solve, and nobody seems to care about the problem they're supposedly solving in the first place.
