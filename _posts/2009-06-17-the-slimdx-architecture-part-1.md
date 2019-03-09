---
layout: post
title: The SlimDX Architecture, Part 1
date: 2009-06-17 10:00
author: ventspace
comments: true
categories: [SlimDX, SlimDX architecture DirectX Direct3D]
---
As I mentioned earlier, there's very little documentation on <strong>how</strong> the internals of SlimDX work, and <strong>why</strong> they work that way. This is important documentation, both for the team and for anyone looking to make changes or add things. I do intend to add full documentation; however, how to lay it out properly isn't quite clear to me. I'm writing this series of blog posts as a dry run to get a sense for what topics are significant and what ordering would make the most sense. I'll try a chronological approach first, and discuss the history of how things used to work.

The original SlimDX code was not intended to be the all-encompassing library it is now. It was just a simplified copy of Managed DirectX, and so it used the same design. The only real difference was that I took out the event stuff. I wasn't using them anyway, due to <a href="http://blogs.msdn.com/tmiller/archive/2003/11/14/57531.aspx">Tom Miller's post</a> about how they were basically kind of dangerous. I wrote SlimDX by tearing MDX out of a game I'd written the previous semester, and retrofitting it with SlimDX until it worked. That game was simple and used a simple resource management model, so there simply was no need to examine the behavior farther. Everything got disposed in its proper way, and that was that.

Needless to say, this isn't a very nice approach to things in the managed world. There was an early attempt to re-enable finalizers, but of course most of DX isn't thread safe and this approach was abandoned quickly. I came up with a stopgap, which was to incorporate registration of every object via the common base class into an <a href="http://code.google.com/p/slimdx/source/browse/trunk/source/ObjectTracker.cpp?r=200">ObjectTracker</a>. 

The common base class was one of the design cues taken directly from MDX, although the idea is relatively straightforward. The original class was even called the same as MDX, DirectXObject. It's not in the code quite from the beginning, but at <a href="http://code.google.com/p/slimdx/source/browse/trunk/source/DirectXObject.h?r=6">r6</a> it's damned close. It eventually gets renamed to BaseObject and then finally ComObject in <a href="http://code.google.com/p/slimdx/source/browse/trunk/source/ComObject.h?r=320">r320</a>. It has been a consistently useful class to have, by incorporating a lot of the basic boilerplate of handling COM objects in a way that makes sense in the managed world. It's a relatively small role for a long time, but later in the SlimDX lifecycle this class becomes one of the most complex and subtle ones we have. The current version is truly terrifying. More on that later.

ObjectTracker was kind of a cool tool, because it meant that at any point in time you could see exactly what objects were outstanding and where they had come from. We even hooked the process exit to report the entire list to the debug spew, so you'd get a nice list of what you leaked. It had a public toggle, so you could turn it off and on as desired. It was added in July of 2007 (<a href="http://code.google.com/p/slimdx/source/browse/?r=110#svn/trunk/source">r110</a>) and the functionality is still there today.

However, although the <em>functionality </em>is still included in SlimDX, the ObjectTracker is long gone. It was replaced in late February 2008 (<a href="http://code.google.com/p/slimdx/source/browse/?r=389#svn/trunk/source">r389</a>) with the <a href="http://code.google.com/p/slimdx/source/browse/trunk/source/ObjectTable.cpp?r=389">ObjectTable</a>. ObjectTable is nearly identical to the old ObjectTracker, but with one fundamental behavior that changed everything. The current SlimDX design is essentially defined by the interactions between the ObjectTable and ComObject classes, which took a <em>very </em>long time to get right. Every little detail is critical, and if you don't understand or pay attentiont to those details -- and we've noticed at least a few people don't -- you're liable to break the library.

I'll go more into depth on those two and why they work the way they do in the next entry.