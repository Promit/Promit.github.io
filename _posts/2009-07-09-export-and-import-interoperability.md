---
layout: post
title: Export and Import Interoperability
date: 2009-07-09 11:00
author: ventspace
comments: true
categories: [Direct3D, DirectX, Interoperability, SlimDX]
---
I've decided that "Windows API Code Pack" is far too long to say/write in casual conversation. WACP it is.<hr>

These are terms I came up with a few weeks ago, and I wanted to document them properly. I feel that they're good descriptions of what a wrapping API like SlimDX allows, and it's useful to be able to settle on common jargon. The basic idea of interoperability is for libraries to be able to cooperate, by sharing objects with each other.

Export interoperability is the ability to "export" objects to other libraries. In the case of SlimDX and similar wrappers, it essentially involves exposing the internal IUnknown pointers, so that another system that supports importing objects can do so. It's not difficult to implement, but it is critical to remember that when your objects are exported, their state can be changed at any time, outside of your control. You can't cache anything that isn't invariant. This is why XNA is unable to do export interop; they chose to cache just about <em>everything</em>, and allowing people to use the underlying interfaces directly will break it. People do it anyway via reflection of course, but it's risky. SlimDX and WACP don't cache anything about the objects, and support export interop cleanly. This is why we can work with libraries like DirectShow.NET, CUDA.NET, and more.

Import interop is of course the ability to consume objects from another library. Of the various DirectX wrappers, SlimDX seems to be the only one that handles import interop. There's no particular reason WACP can't do it, as far as I can tell; they simply haven't added it to the public interface. With XNA, I believe that the cached values are again the problem, I suspect. They could be looked up on construction, but somebody else still has direct access to the interface. Import interop is at its core not that clever, because it's a basic part of building the wrapper in the first place. You have to be able to convert pointers from the unmanaged API to your own objects, so it's not a big step to do it from arbitrary pointers. The trick is doing it safely; you have to trust that the pointer you're given is what the caller says it is. SlimDX assumes it is an IUnknown and then uses QueryInterface to get the desired type. This is our only line of defense, but it's a fairly effective one.

Interoperability has been a major focus for the SlimDX design, for both import and export. There's a fair amount of complexity involved in object construction, but it's been carefully laid out to be able to handle external pointers. There was some internal caching of values early on, which we thought were invariant, but we were seeing some difficult to track problems and so we backed out the code to a safe implementation. Our commitment to making sure we can both export and import objects goes far beyond the other major wrappers,  although WACP should be able to provide equivalent functionality if they stick to the current design or similar.
