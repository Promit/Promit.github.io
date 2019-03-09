---
layout: post
title: DirectX/XNA Phase Out Continues
date: 2013-01-30 12:50
author: ventspace
comments: true
categories: [Graphics, SlimDX]
---

---
<b><a href="http://t.co/ILyyzXQK">Please read the follow up post.</a></b>
---

This email was sent out to DirectX/XNA MVPs today:
<blockquote>The XNA/DirectX expertise was created to recognize community leaders who focused on XNA Game Studio and/or DirectX development. Presently the XNA Game Studio is not in active development and DirectX is no longer evolving as a technology. Given the status within each technology, further value and engagement cannot be offered to the MVP community. As a result, effective April 1, 2014 XNA/DirectX will be fully retired from the MVP Award Program.</blockquote>
There's actually a fair bit of information packed in there, and I think some of it is poorly worded. The most stunning part of it was this: "DirectX is no longer evolving as a technology." That is a phrase I did not expect to hear from Microsoft. Before going to "the sky is falling" proclamations, I don't think this is a death sentence for DirectX, per se. It conveys two things. Number one, DirectX <i>outside of Direct3D</i> is completely dead. I hope this is not a shock to you. Number two, it's a reminder that Direct3D has been absorbed into Windows core, and thus is no more a "technology" than GDI or Winsock.

Like I said, poorly worded.

There are a few other things packed in there. XNA Game Studio is finished. That situation has been obvious for years now, so it also should not really come as a surprise either. And finally the critical point for me: our "MVP" role as community representatives and assistants is appreciated but no longer necessary. On this point, the writing has been on the wall for some time and so I should not be surprised. But I am. Maybe dismayed is a better word.

As I've said previously, I don't feel that the way DirectX has been handled in recent years has been a positive thing. A number of technical decisions were made that were unfortunate, and then a number of business and marketing type decisions were made that compounded the problem. Many of the technologies (DirectInput, DirectSound, DirectShow) have splayed into a mess of intersecting fragments intended to replace them. The amount of developer support for Direct3D <i>from Microsoft</i> has been unsatisfactory, and anecdotal reports of internal team status have not been promising. Somebody told me a year or two back that the HLSL compiler team was one person. That's not something you want to hear, true or not. Worst of all, though, was the communication. That's the part that bugs me.

When you are in charge of a platform, whatever that platform may be, developers invest in your platform tech. That's time and money spent, and opportunity costs lost elsewhere. This is an expected aspect of software development. As developers and managers, we want as much information as possible in order to make the best short and long term decisions on what to invest in. We don't want to rewrite our systems from scratch every few years. We don't want to fall behind competitors due to platform limitations. Navigating these pitfalls is crucial to survival for us. Microsoft has a vested interest in some level of non-disclosure and secrecy about what they're doing. All companies do. I understand that. But some back and forth is necessary in order for the relationship to be productive.

Look at XNA -- there have been a variety of questions surrounding it for years, about the extent to which the technology and its associated marketplace were going to be taken seriously and forward into the future. It is clear at this juncture that there was no future and the tech was being phased out. Direct3D 10 was launched in late 2006, a bit over six years ago, yet XNA was apparently never going to be brought along with the major improvements in DWM and Direct3D. How long was it known internally at Microsoft that XNA was a dead-end? How many people would've passed over XNA if MS had admitted circa 2008 (or even 2010, when 4.0 was released) that there was no future for the tech? The official response, of course, was always something vague and generic: "XNA is a supported technology." That means nothing in Microsoft world, because "it will continue to work in its current state for a while" is not a viable way for developers to stay current with their competition.

Just to be clear, I don't attribute any of this fumbling to malice or bad faith. There's a lot of evidence that this type of behavior is merely a delayed reflection of internal forces at Microsoft which are wreaking havoc on the company's ability to compete in <i>any</i> space. But the simple ground truth is that we're entering an era where Windows' domination is openly in question, and a lot of us have the flexibility and inclination to choose between a range of platforms, whether those platforms are personal computers, game consoles, or mobile devices. Microsoft's offer in that world is lock-in to Windows, in exchange for powerful integrated platforms like .NET which are far more capable than their competitors (eg Java, which is just pathetic). That was an excellent trade-off for many years. Looking back now, though? The Windows tech hegemony is a graveyard. XNA. Silverlight. WPF. DirectX. Managed C++. C++/CLI. Managed DirectX. Visual Basic. So when you guys come knocking and ask us to commit to Metro -- sorry, the <i>Windows 8 User Experience</i> -- and its associated tech?

You'll understand if I am not in a hurry to start coding for your newest framework.

<i>Before things get out of hand: No, you should not switch to OpenGL. I get to use it professionally every day and it sucks. Direct3D 11 with the Win8 SDK is a perfectly viable choice, much more so than OpenGL for high end development. None of the contents of my frequent complaints should imply in any way that OpenGL is a good thing.</i>
