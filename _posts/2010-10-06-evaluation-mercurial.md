---
layout: post
title: Evaluation: Mercurial
date: 2010-10-06 11:13
author: ventspace
comments: true
categories: [dvcs, git, mercurial, Software Engineering, Subversion, SVN, TortoiseHg]
---
I've been a long time Subversion user, and I'm very comfortable with its quirks and limitations. It's an example of a centralized version control system (CVCS), which is very easy to understand. However, there's been a lot of talk lately about <i>distributed</i> version control systems (DVCS), of which there are two well known examples: git and Mercurial. I've spent a moderate amount of time evaluating both, and I decided to post my thoughts. This entry is about Mercurial.

<b>Short review:</b> A half baked, annoying system.

I started with Mercurial, because I'd heard anecdotally that it's more Windows friendly and generally nicer to work with than git. I was additionally spurred by reading the first chapter of <a href="http://hginit.com/">HgInit</a>, an e-book by Joel Spolsky of 'Joel on Software' fame. Say what you will about Joel -- it's a concise and coherent explanation of why distributed version control is, in a general sense, preferable to centralized. Armed with that knowledge, I began looking at what's involved in transitioning from Subversion to Mercurial.

Installation was smooth. Mercurial's site has a Windows installer ready to go that sets everything up beautifully. Configuration, however, was unpleasant. The <a href="http://mercurial.selenic.com/guide/">Mercurial guide</a> starts with this as your very first step:
<blockquote>As first step, you should teach Mercurial your name. For that you open the file ~/.hgrc with a text-editor and add the ui section (user interaction) with your username:</blockquote>
Yes, because what I've always wanted from my VCS is for it to be a hassle every time I move to a new machine. Setting up extensions is similarly <a href="http://mercurial.selenic.com/wiki/UsingExtensions">a pain in the neck</a>. More on that in a moment. Basically Mercurial's configurations are <A href="http://www.selenic.com/mercurial/hgrc.5.html">a headache</a>.

Then there's the actual VCS. You see, I have one gigantic problem with Mercurial, and it's summed up by Joel:
<blockquote>Whereas, in Mercurial, all commands always apply to the entire tree. If your code is in c:\code, when you issue the hg commit command, you can be in c:\code or in any subdirectory and it has the same effect.</blockquote>This is an <i>incredibly</i> awkward design decision. The basic idea, I guess, is that somebody got really frustrated about forgetting to check in changes and decided this was the solution. My take is that this is a stupid restriction that makes development unpleasant.

When I'm working on something, I usually have several related projects in a repository. (Mercurial fans freely admit this is a bad way to work with it.) Within each project, I usually wind up making a few sets of parallel changes. These changes are independent and shouldn't be part of the same check-in. The idea with Mercurial is, I think, that you simply produce new branches every time you do something like this, and then merge back together. Should be no problem, since branching is such a trivial operation in Mercurial.

So now I have to stop and think about whether I should be branching every time I make a tweak somewhere?

Oh but wait, how about the extension mechanism? I should be able to patch in whatever behavior I need, and surely this is something that bothers other people! As it turns out that <a href="http://stackoverflow.com/questions/3012928/doing-without-partial-commits-the-mercurial-way">definitely the case</a>. Apart from the branching suggestions, there's not one but <a href="http://mercurial.selenic.com/wiki/PatchHandlingUnificationRFC">half a dozen extensions</a> to handle this problem, all of which have their own quirks and pretty much all of which involve jumping back into the VCS frequently. This is apparently a problem the Mercurial developers are still puzzling over.

Actually there is one tool that's solved this the way you would expect: <a href="http://tortoisehg.bitbucket.org/">TortoiseHg</a>. Which is great, save two problems. Number one, I want my VCS features to be available from the command line and front-end both. Two, <a href="http://ventspace.wordpress.com/2009/12/22/my-favorite-svn-tool-smartsvn/">I really dislike Tortoise</a>. Alternative Mercurial frontends are both trash, and an unbelievable pain to set up. If you're working with Mercurial, TortoiseHg and command line are really your only sane options.

It comes down to one thing: workflow. With Mercurial, I have to be constantly conscious about whether I'm in the right branch, doing the right thing. Should I be shelving these changes? Do they go together or not? How many branches should I maintain privately? Ugh. 

Apart from all that, I ran into one serious show stopper. Part of this test includes migrating my existing Subversion repository, and Mercurial includes a convenient <a href="http://mercurial.selenic.com/wiki/ConvertExtension">extension for it</a>. Wait, did I say convenient? I meant <a href="http://mercurial.selenic.com/wiki/ConvertExtension#Converting_from_Subversion">borderline functional</a>:
<blockquote>Subversion's Python bindings are a prerequisite. The bindings (generated with SWIG) are installed separately on Windows, and can be found on http://subversion.tigris.org/ . Note that you can't do this with the Win32 Mercurial binaries -- there's no way to install the Subversion bindings into its built-in Python library. So you'll need to use a Mercurial installed on top of a stand-alone Python, and you may also need to do something like "set HG=python c:\Python25\Scripts\hg" to override the default Win32 binaries if you have those installed also. For Mac OS X, the easiest way is to install the CollabNet Subversion build, and then copy the content of /opt/subversion/lib/svn-python to the site-package directory of the python installation.</blockquote>
The silver lining is there are apparently third party tools to handle this that are far better, but at this point Mercurial has tallied up a lot of irritations and I'm ready to move on.

<b>Spoiler:</b> I'm transitioning to git. I'll go into all the gory details in my next post, but I found git to be vastly better to work with.
