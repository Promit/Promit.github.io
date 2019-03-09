---
layout: post
title: ClickOnce Support in SlimDX
date: 2010-02-19 19:29
author: ventspace
comments: true
categories: [SlimDX]
---
Man, it's been a long time since I wrote about SlimDX. We've released the February 2010 version today, so go ahead and grab that if you're so inclined. This version is mostly bug fixes, for both us and Microsoft. DirectX 11 should be much more usable, although we're still working towards stronger D2D and DWrite implementations. In the meantime, I wanted to discuss a feature that was included too late for documentation to catch up: ClickOnce support.

ClickOnce has actually been on the to-do list for a very long time, but it ended up quite far down the priority list thanks to a lack of user demand. For the February 2010 release we've gone ahead and included it. The installer will set it up for VS 2008 and VS 2010; we've dropped 2005 support across the board so no ClickOnce there. There are a few quirks to setting it up properly though, so I just want to explain what you need to do in order to make sure it works properly.

First of all, make sure you've got a reference to the GAC version of SlimDX in your project (via the .NET tab in Add References). Also check the properties of the SlimDX reference; the default is Copy Local = True and that should be set to False.

Once you've done that, go into your project's properties and select the Publish tab.  This is where you set up all of the ClickOnce options and actually produce a distribution. If you press Application Files, you should see SlimDX.dll listed as Prerequisite (Auto). If it's something else, you've got the previous step wrong.

Next, hit the Prerequisites... button. Somewhere in the listbox, you'll see "SlimDX Runtime (February 2010)". Check the box and press OK.

That's it. Pretty easy, huh? Now when you publish, the SlimDX runtime will be included with your application and run automatically as part of the ClickOnce installation.
