---
layout: post
title: SlimDX's Hobbyist Curse, and Open Source Blessing
date: 2009-06-13 10:00
author: ventspace
comments: true
categories: [SlimDX]
---
There are two attributes that fundamentally differentiate SlimDX from MDX and XNA:
<ul>
	<li>It's a "hobbyist" project.</li>
	<li>It's open source.</li>
</ul>

The Windows API Code Pack mucks this up a bit, but more on that another time. I've already opened my mouth about it once without knowing what I was talking about, so I'll actually do my due diligence this time.

Our status of being "hobbyists" is a problem for some people. This is a trend I usually observe on the MS newsgroups and Stack Overflow. Usually the thinking is that XNA and MDX, by virtue of being written by Microsoft, are better supported, less likely to die out, and generally safer to use. The snarky thing to do at this point is to ask how MDX 2.0 worked out for them, but generally speaking if you're trying to pull someone to your side, making fun of them is not terribly effective.

Of course the people who are open minded, who know us, etc don't have this problem. It's well known in our circles (GameDev, mostly, and XNA to a lesser extent) that we're professional developers who know what we're doing. There's a fair argument to be made that you don't <em>want</em> the other kind of user, but it's difficult not to bristle a bit when you read a post by someone who doesn't want to use the library because it's not official.

Our open source status is in many ways the saving grace, though. When I started the project around January of 2006, it was a very easy decision that the source code had to be readily accessible and usable. I settled on the MIT license because it essentially sets no restrictions on what you can do with the library. Not only are there no restrictions on usage, but you can basically do whatever you want with any of the source code. The idea was, of course, that people could add the functionality they needed (since the original had almost none) and everybody would be happy with their own variations on SlimDX.

SlimDX's scope grew from a little personal wrapper to a full replacement project, but that element of customization has been quite powerful. A number of our users, especially the corporate ones like <a href="http://www.ventuz.com/index.aspx">Ventuz</a>, use customized builds that work better for them, for whatever reason. It also allows them to fix bugs as soon as they're identified, instead of waiting for us to get around to it. (Sadly this usually means we don't find out about those bugs.)

And, as I've always told some of the doubters, the source code is hosted on Google servers with every revision ever, and requires just a few very common bits to build. Even if we "hobbyists" give it up, so what? It's not like MDX 2.0, where the binaries time bomb and vanish, and the samples dissolve into thin air. You can even implement DirectX 12 or whatever it turns out to be, and you'll still benefit from all of our architecture and basically stable codebase. At least, you could <em>in theory</em>. In practice, there's one problem -- nearly all of the internal documentation is in our heads. I'll be working to change that over the coming weeks, right here. And yes, new documentation will show up as well.
