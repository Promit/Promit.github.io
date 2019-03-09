---
layout: post
title: SlimDX Supports Direct3D 10.1, too
date: 2009-06-25 10:00
author: ventspace
comments: true
categories: [d3d, d3d 10, Direct3D, direct3d 10, DirectX, directx 10, SlimDX]
---
<a href="http://slimdx.org">SlimDX</a> has had Direct3D 10 support for a long time, and it's almost certainly the single best way to get 10 support from C# or any other .NET language. While it's true that the very original prototype didn't plan on it, that was over two years ago and it's very much a first class citizen. Although we have Direct3D 11 support now (see <a href="http://ventspace.wordpress.com/2009/06/09/c-and-directx-11-yes-you-can/">this post</a>), 10 is still a priority and we're not leaving it behind.

As part of that commitment, SlimDX now has full Direct3D 10.1 support. It will be part of our next release, which looks like it will be August 09 at this point. Although nobody specifically asked for 10.1, it's a very useful API in its own right. Sure it adds some features, but just like DirectX 11, it has <em>feature levels</em>. In other words, even though DirectX 10 requires DX10 class hardware, 10.1 works on DX9 or 10 hardware! The caveat is that Vista SP1 or later is required, but if that's an acceptable limitation, 10.1 is great to have for that reason alone.

And as always, if you find missing features or bugs or whatever, let us know! D3D 10 is in use by several of our customers in production environments, and we're determined to continue being the single best option for using it from C#, VB.NET, or anything else in the .NET ecosystem.
