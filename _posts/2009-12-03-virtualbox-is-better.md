---
layout: post
title: VirtualBox is Better (wait, maybe not)
date: 2009-12-03 13:53
author: ventspace
comments: true
categories: [Software Engineering, virtualbox, virtualization, virtualpc, vmware]
---
So after yesterday, I figured I may as well give VirtualBox its fair shake. It's difficult to find up-to-date information on ANY of the virtualization systems, as most of them have gained a whole slew of features recently. I discovered that VMWare Player can do hardware accelerated graphics, but only for Windows guests. Presumably this is a result of their focus on the Fusion product. I've also installed VirtualBox and played with a bit, and initial impressions are pretty good.

Now, I haven't run any performance tests, but cursory searching suggests that VirtualBox performance is probably not as good as VMWare, but in running things I haven't noticed any problems. What I'm doing isn't very resource intensive, for the most part. And unlike VMWare, VirtualBox supports graphics acceleration for both Linux and Windows guests, OpenGL and Direct3D. Not a bad start.

Since VirtualBox is entirely free software as well as open source, there's no particular missing features either. The latest version has added a bunch of cool stuff, including fairly sophisticated snapshot support. I'm still playing with it, but so far things are looking good. I'll probably follow up in a few weeks with more comments.

I suppose though that the real takeaway from all of this is simple -- don't use Virtual PC <del datetime="2009-12-04T21:26:16+00:00">or VMWare Server</del>.
<strong>UPDATE:</strong> I want to highlight a comment posted by bubu on the previous entry:
<blockquote>VMWare Server version 2.x and not web-interface based. It is pretty usable and is still available as download from VMWare site. It also supports snapshots and are available for running over network.</blockquote>
<strong>UPDATE 2:</strong> I tried to enable USB in VirtualBox and it kinda blew up. It's not at all smooth the way VMWare and VirtualPC handle it, so I'm back to Player.
