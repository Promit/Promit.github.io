---
layout: post
title: Follow-up on DirectX/XNA
date: 2013-01-31 17:45
author: ventspace
comments: true
categories: [SlimDX]
---
Received today, and hopefully the "you can quote me" part means this is an exception to NDA because it's important:
<blockquote>The message said “DirectX is no longer evolving as a technology.” That is definitely not true in any way, shape or form. Microsoft is actively investing in DirectX as the unified graphics foundation for our key platforms, including Xbox 360, Windows Phone and Windows. DirectX is evolving and will continue to evolve. For instance, right now we’re investing in some very cool graphics code authorizing [sic] technology in Visual Studio. We have absolutely no intention of stopping innovation with DirectX, and you can quote me on that. :)</blockquote>
My intent was not to start a firestorm of questioning on DirectX's future viability, and I said up-front that I felt that communication was poorly worded with regards to intent. My frustrations were also apparently poorly worded. Since I accidentally launched this, let's clear up a few things.

<b>Number One:</b> In the absolute (and implausible) worst case scenario that MS really scales back their Direct3D support to a minimum, that situation is <i>still</i> better than OpenGL. The Direct3D system is a technically superior piece of technology, and support for working with it is still better than OpenGL whether you're a hobbyist or a pro. I cannot emphasize this point enough, so for the love of god stop bringing up OpenGL. It's a badly designed API and has been since I started doing this in 2000.
<b>Number Two:</b> A new picture is coming into focus that shifts a lot of the DirectX SDK's burden onto VS. This hasn't been made previously clear to us on the MVP side. As I've begun to explore the tools already inside VS 2012, I like what I'm seeing. It'll take some time to see how it all plays out, but in a very real way having Direct3D integrated into core VS development is a serious promotion.
<b>Number Three:</b> There's more content in today's email regarding XNA which I don't care to share, thanks to a stern NDA reminder. (Ironically, when MS finally gives us what they should be saying to the public all along, I can't share it.) But this is very much a case of "put up or shut up" and defending XNA's status as a serious technology seems patently ridiculous to me right now. The community, whether it's my work or someone else's, has stepped in to integrate .NET and DirectX for many wonderful use cases. But there are things we can't do (like Xbox) and it's clear that matters to a lot of people. It's not clear that it matters to Microsoft.

<i>That said</i>, I am not walking back my actual complaints about how DirectX and XNA are being handled. I like the work that's been done in integrating VS and DirectX, which is arguably many years overdue. That doesn't make everything else okay. The fact that we're having this discussion, the fact that my dashed off blog post exploded on Twitter, the fact that clarification had to be written up behind the scenes -- <i>this is a problem</i>. Which brings me at long last to the actual point I was trying to make yesterday:

<i>As developers, we need Microsoft to communicate clearly with us, <b>in public</b>.</i> As MVPs we were asked to act as community representatives, to guide everyone interested in the tech and have an open line on future development. Apparently that means we get half-hearted vague emails from time to time that dodges our serious questions and casts further doubts about the status of the technology and teams, all covered by an NDA agreement. And then, shockingly enough, people get the wrong idea. We're sitting on the outside, trying to play this stupid guessing game of "which Microsoft technology is alive?" XNA doesn't support DirectX 10+ or Windows 8, but it's still a "supported product", as if that means anything in the real world. Windows XP is still a "supported product" too.

It shouldn't take a leaked email to force a straight answer.
