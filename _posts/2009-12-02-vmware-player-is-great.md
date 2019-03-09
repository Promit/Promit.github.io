---
layout: post
title: VMWare Player is Great
date: 2009-12-02 19:42
author: ventspace
comments: true
categories: [Software Engineering, virtualization, virtualpc, vmware]
---
Recently, I've started using VMWare Player. The original VMWare Player was a crippled piece of junk that could only play pre-made virtual machine images, and represented one of the very early volleys in the free virtualization war. As a result, most of the readily found information about Player on the net is completely wrong, and I think it's not a generally appreciated product because of it.

There are a number of virtualization competitors with free products, most of which I haven't used because they tend not to quite stack up to the big players in the market, of which there are three I know about: VMWare, Microsoft, and VirtualBox. VMWare is considered state of the art, and with good reason. There are a few different versions of their software, all of which are fantastically capable. Microsoft made waves by purchasing their VM technology from Connectix and making it free under the name Virtual PC. There's also VirtualBox, which is free and open source, but I haven't worked with. I probably should, but it's owned by Sun Microsystems so I have a tough time believing it doesn't suck.

Virtual PC is decent, not stellar. I use it for SlimDX development, since it works well with Windows and has a decent set of features. It's being pulled into Windows 7 as "XP mode", and there's a new version of it for Windows 7 as well. This new version (which is beta or RC, I forget) is poor. The management interface is integrated into Explorer for some reason, and I've had lots of glitches with it. It also has some weird shared USB mode that doesn't seem to actually work, but you can ask it to take over USB entirely.

VMWare Workstation has long been a favorite of mine, and at around USD 130 it's a highly affordable software package. Still, it's $130 I don't have to spend, and so I stick to free options. VMWare released a product called Server for free some years ago. Server sucks. The interface is a damned web app, I don't know why, but it's really awkward to use. The VM runs as a service, which is inconvenient, and the actual VM window has to be opened from your browser as well, which kicks off a plugin that never seems to quite work right. 

That brings us to VMWare Player. Like I said, the original version was crippled -- but this one has practically everything. Multicore support is there, which Virtual PC doesn't have. You can create and manage VMs quite well. VM suspend and resume is available, and even Unity is enabled. The only omission I've noticed compared to Workstation is that snapshots are not supported at all. This is a bit unfortunate. VPC has a very limited form of snapshots, where you're allowed to abandon all changes made since the VM was started. I use that feature quite a bit for SlimDX development, so I'll still be using VPC from time to time. VMWare also doesn't include the radical debugging features and integration that the Workstation product does, but I don't think that's too popular in general. I also gather Workstation has 3D support these days, but I've never attempted to use it. Player's support for 3D is minimal, if any. VPC has no 3D support at all, which is a real pain for doing SlimDX work.

It's no good for gaming, but for development and running virtual servers, Player is a fabulous product. 
