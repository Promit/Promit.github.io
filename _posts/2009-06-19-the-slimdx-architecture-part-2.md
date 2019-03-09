---
layout: post
title: The SlimDX Architecture, Part 2
date: 2009-06-19 10:00
author: ventspace
comments: true
categories: [SlimDX, SlimDX DirectX Direct3D]
---
Now we get into the real substance of things. In <a href="http://ventspace.wordpress.com/2009/06/17/the-slimdx-architecture-part-1/">Part 1</a>, I discussed some of the historical elements that went into the design of ComObject and ObjectTable, but stopped short of explaining the current system. In this segment, I'll cover the essentials of how things work today. You should also read <a href="http://scientificninja.com/development/com-and-slimdx-part-i">COM and SlimDX, Part I</a>, as it does a better job than me of covering some of the details. (Remember, these are test drafts for new documentation.)

As SlimDX and its userbase grew, a number of cracks began to show up in the design. At the heart of things was that a lot of new objects were created every time you tried to do something, which meant a lot of things to dispose. Sure we had no problems with events chaining endlessly, but even with the leak tracker it was a real pain in the ass to make sure everything was handled properly. As a result, we also had to be very, very clear about where objects were created. With an object such as Texture that allows you to get its parent Device, a Device property was out of the question. It had to be GetDevice(), or the amount of stealthy object creations could get out of hand very quickly -- and for no apparent reason.

Consider what a function like GetDevice() has to do. The native API gives us an IDirect3DDevice9*, which we have to convert into a managed reference to Device. The obvious way to do this -- the way MDX worked -- is to create a new Device object, passing it the pointer to be used internally. The device has already had the COM AddRef() function called to adjust the reference count, and the reference count is restored when Dispose() is called. It's a simple and efficient strategy to implement, and creates a huge amount of allocations. We referred to this as the GetDevice() != GetDevice() problem at the time, because we didn't implement equality operators and so the new objects didn't even compare to the same thing.

<a href="http://scientificninja.com/">Josh</a> had the initial idea to address this. I never did research this in depth, but when you ask for Visual Studio to add a reference to a COM object, it creates something called a <a href="http://msdn.microsoft.com/en-us/library/f07c8z1c.aspx">COM callable wrapper</a>. Apparently the wrapper uses some kind of table of objects; I don't really know the details. The basic idea, though, was to construct a table of mappings from native pointers to managed instances. By switching over to factory construction for most objects internally, we were able to make the table work.

ObjectTracker became ObjectTable, and maintains a Dictionary of IntPtr -&gt; ComObject. Internally, when we call a method that always creates a new object (eg D3DXCreateTextureFromFile), the new object is added to the table. For methods that might return an object that is already in the table, we check the table to see if it contains the pointer we just got. If it does, the table <em>calls Release on the object</em> and then returns the original managed instance. When an object is disposed, we call Release and then <em>remove it from the table</em>. It's set up this way in ordeto maintain a very important invariant:
<strong>SlimDX is always responsible for at most one reference on a COM object.</strong>

By maintaining this invariant, lifetime management is much easier to control. Instead of having to match up a large list of AddRef and Release calls to make sure everything comes together cleanly, all you have to do is Dispose the object once, usually the first reference to it that you saw. Stuff like GetDevice() became properties, which return the original instance without any caveats and without creating memory management headaches. Actually, that's not quite true -- there is one caveat if you're doing interop. <a href="http://scientificninja.com/development/com-and-slimdx-part-ii">COM and SlimDX, Part II</a> has a proper explanation.

There is one more issue outstanding with the ObjectTable, which Josh will write up for me soon. For part 3 of my series, I'll step away from this stuff and take a look at DataStream.
