---
layout: post
title: Windows Installer is Terrible
date: 2010-06-30 22:57
author: ventspace
comments: true
categories: [msi, Software Engineering, windows installer]
---
I find Windows Installer to be truly baffling. It's as close to the heart of Windows as any developer tool gets. It is technology which literally every single Windows user interacts with, frequently. I believe practically every single team at Microsoft works with it, and that even major applications like Office, Visual Studio, and Windows Update are using it.

So I don't understand. Why is Installer such a poorly designed, difficult to use, and generally infuriating piece of software?

Let's recap on the subject of installers. An installer technology should facilitate two basic tasks. One, it should allow a developer to smoothly install their application onto any compatible system, exposing a UI that is consistent across every installation. Two, it should allow the user to completely reverse (almost) any installation at will, in a straightforward and again consistent fashion. Windows, Mac OSX, and Linux take three very different approaches to this problem, with OSX being almost indisputably the most sane. Linux is fairly psychotic under the hood, but the idea of a centralized package repository (almost like an "app store" of some kind) is fairly compelling and the dominant implementations are excellent.

And then we have Windows. The modern, recommended approach is to use MSI based setup files, which are basically embedded databases and show a mostly similar UI. And then there's InstallShield, NSIS, InnoSetup, and half a dozen other installer technologies that are all in common use. Do you know why that is? It's because Windows Installer is junk.

Let us start with the problem of consistency. This is our very nice, standard looking SlimDX SDK installation package:
<img src="http://ventspace.files.wordpress.com/2010/06/slimdx-installer.png" alt="" title="SlimDX Installer" width="505" height="392" class="alignnone size-full wp-image-606" />
And this is what it looks like if you use Visual Studio to create your installer:
<img src="http://ventspace.files.wordpress.com/2010/06/vs-installer.png" alt="" title="VS Installer" width="509" height="414" class="alignnone size-full wp-image-609" />
Random mix of fonts? Check. Altered dialog proportions for no reason? Check. Inane question that makes no sense to most users? Epic check. Hilariously amateur looking default clip art? Of course.

Okay, so maybe you don't think the difference is that big. Microsoft was never Apple, after all. But how many of those childish looking VS based installers do you see on a regular basis? It's not very many. That's because the installer creation built into Visual Studio, Microsoft's premiere idol of the industry development tool, is utter garbage. Not only is the UI for it awful, it fails to expose most of the useful things MSI can actually do, or most developers want to do. Even the traditionally expected "visual" half-baked dialog editor never made it into the oven. You just get a series of bad templates with static properties. Microsoft also provides an MSI editor, which looks like this:
<img src="http://ventspace.files.wordpress.com/2010/06/orca-msi.png" alt="" title="Orca MSI editor" width="692" height="479" class="alignnone size-full wp-image-624" />
Wow! I've always wanted to build databases by hand from scratch. Why not just integrate the functionality into Access?

In fact, Microsoft is now using <i>external</i> tools to build installers. Office 2007's installer is written using the open source <a href="http://wix.sourceforge.net/">WiX</a> toolset. Our installer is built using WiX too, and it's an unpleasant but workable experience. WiX essentially translates the database schema verbatim into an XML schema, and automates some of the details of generating unique IDs etc. It's pretty much the only decent tool for creating MSI files of any significant complexity, especially if buying InstallShield is just too embarrassing (or expensive, $600 up to $9500). By the way, Visual Studio 2010 now includes a license for InstallShield Limited Edition. I think that counts as giving up.

Even then, the thing is downright infuriating. You cannot tell it to copy the contents of a folder into an installer. There is literally no facility for doing so. You have to manually replicate the entire folder hierarchy, and every single file, interspersed with explicit uniquely identified Component blocks, all in XML. And all of those components have to be explicitly be referenced into Feature blocks. SlimDX now ships a self extracting 7-zip archive for the samples mainly because the complexity of the install script was unmanageable, and had to be rebuilt with the help of a half-baked C# tool each release.

Anyone with half a brain might observe at this point that copying a folder on your machine to a user's machine is mostly what an installer does. In terms of software design, <i>it's the first god damned use case</i>.

Even all of that might be okay if it weren't for one critical problem. Lots of decent software systems have no competent toolset. Unfortunately it turns out that the underlying Windows Installer engine is <i>also</i> a piece of junk. The most obvious problem is its poor performance (I have an SSD, four cores, and eight gigabytes of RAM -- what is it doing for so long before installation starts?), but even that can be overlooked. I am talking about one absolutely catastrophic, completely unacceptable design flaw. 

Windows Installer cannot handle dependencies.

Let that sink in. Copying a local folder to the user's system is use case number one. Setting up dependencies is, I'm pretty sure, the very next thing on the list. And Windows Installer cannot even begin to contemplate it. You expect your dependencies to be installed via MSI, because it's the standard installer system, and they usually are. Except...Windows Installer can't chain MSIs. It can't run one MSI as a child of another. It can't run one MSI after another. It sure can't conditionally install subcomponents in separate MSIs. Trying to run two MSI installs at once on a single system will fail. (Oh, and MS licensing doesn't even allow you to integrate any of their components directly in DLL form, the way OSX does. Dependencies are MSI or bust.)

The way to set up dependencies is to write your own custom bootstrap installer. Yes, Visual Studio can create the bootstrapper, assuming your dependencies are one of the scant few that are supported. However, we've already established that Visual Studio is an awful choice for any installer-related tasks. In this case, the bootstrapper will vomit out five mandatory files, instead of embedding them in setup.exe. That was fine when software was still on media, but it's ridiculous for web distribution.

Anyway, nearly any interesting software requires a bootstrapper, which has to be pretty much put together from scratch, and there's no guidelines or recommended approaches or anything of the sort. You're on your own. I've tried some of the bootstrap systems out there, and the best choice is actually <i>any competing installer technology</i> -- I use Inno. Yes, the best way to make Windows Installer workable is to actually wrap it in a third party installer. And I wonder how many bootstrappers correctly handle silent/unattended installations, network administrative installs, logging, UAC elevation, patches, repair installs, and all the other crazy stuff that can happen in installer world.

One more thing. The transition to 64 bit actually made everything worse. See, MSIs can be built as 32 bit or 64 bit, and of course 64 bit installers don't work on 32 bit systems. 32 bit installers are capable of installing 64 bit components though, and can be safely cordoned off to exclude those pieces when running on a 32 bit system. Except when they can't. I'm not sure exactly how many cases of this there are, but there's one glaring example -- the Visual C++ 2010 64 bit merge module. (A merge module is like a static library, but for installers.) It can't be included in a 32 bit installer, even though the VC++ 2008 module had no problem. The recommended approach is to build completely separate 32 and 64 bit installers.

Let me clarify the implications of that statement. Building two separate installers leaves two choices. One choice is to let the user pick the correct installation package. What percentage of Windows users do you think can even understand the selection they're supposed to make? It's not Linux, the people using the system don't know arcane details like what bit-size their OS installation is. (Which hasn't stopped developers from asking people to choose anyway.) That leaves you one other choice, which is to -- wait for it -- write a bootstrapper.

Alright, now I'm done. Despite all these problems, apparently developers everywhere just accept the status quo as perfectly normal and acceptable. Or maybe there's a "silent majority" not explaining to Microsoft that their entire installer technology, from top to bottom, is completely mind-fucked.
