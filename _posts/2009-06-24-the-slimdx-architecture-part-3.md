---
layout: post
title: The SlimDX Architecture, Part 3
date: 2009-06-24 10:00
author: ventspace
comments: true
categories: [SlimDX, Software Architecture]
---
I asked Josh to write up a follow up to the first two parts, covering the ancillary object support in our ObjectTable, which he graciously agreed to. <a href="http://scientificninja.com/development/ancillary-objects-in-slimdx">Here it is</a>.
<hr>

I want to switch gears a little bit and talk about a class you've almost certainly seen if you're doing any SlimDX work: <strong>DataStream</strong>. I'd describe ObjectTable and ComObject as the heart of SlimDX, and DataStream as the soul. It's been exceedingly difficult to get right, with 41 revisions of the header and 50 revisions of the source, even though the feature set has only changed a little bit over time. In fact, we just changed it again. It's critical to most people because it does the one thing everybody needs to do -- transfer data from the application to the API, and occasionally vice versa.

The original class in MDX was called GraphicsStream, and SlimDX used the same name for a while. We decided in <a href="http://code.google.com/p/slimdx/source/detail?r=138">r138</a> to rename the class to DataStream, since it was fairly obvious that it wasn't graphics-specific in any way. The name was never quite ideal, but MemoryStream is taken and we couldn't figure out something better. Besides, it's kinda catchy. Although the goal of the class was kind of vague at first, it fundamentally represents what a pointer has become in managed code. It's not quite as elegant in many ways, but unlike a pointer it is a hell of a lot safer.

The DataStream typically replaces a pointer in the underlying API, but a pointer in the unmanaged world is a relatively simple beast. In the managed world, we have a whole host of problems. There's a few different places that a user could want to copy data to or from:<ul>	<li>An array on the managed heap</li>
	<li>A managed Stream</li>
	<li>A buffer on the native heap (owned by anybody)</li>
	<li>Memory in an ID3DXBuffer</li>
</ul> In XNA, the approach to handling this complexity is to dispense with it entirely, and limit data transfers to managed arrays only. I criticized this decision heavily when the beta was released, but it's not necesarily an unreasonable one. I just didn't think it was the right move. It does fit in with the overall design strategy of XNA, after all. However, it doesn't fit the SlimDX design philosophy, so we have a somewhat different approach.

DataStream can do <em>all</em> of these things. That's why the name is so vague; trying to quantify the class at all is difficult. It exists to mediate data transfer between an application and DirectX, in either direction; it provides the standard Stream interface. Every DataStream has a backing store that can be stored in a pointer, but sometimes there's auxiliary members. (A pin handle or an ID3DXBuffer are currently supported.) You can even allocate memory off the native heap for use via one of the constructors, in case that's useful.

DataStream's code is very simple and easy to understand, but it's fraught with subtleties. The bounds checks have to be exactly right, which has been surprisingly difficult to do. It's also important to use the correct pointer; sometimes this is the beginning of the stream and sometimes it's the stream position. Despite those pitfalls, it's not a class that looks threatening...but it does power nearly all of the data transfer in SlimDX.
