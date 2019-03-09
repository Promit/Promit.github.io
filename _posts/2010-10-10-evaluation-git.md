---
layout: post
title: "Evaluation: Git"
date: 2010-10-10 18:59
author: ventspace
comments: true
categories: [dvcs, git, mercurial, Software Engineering, Subversion, SVN]
---
Last time I talked about Mercurial, and was generally disappointed with it. I also evaluated Git, another major distributed version control system (DVCS). 

<b>Short Review:</b> Quirky, but a promising winner.

Git, like Mercurial, was spawned as a result of the Linux-BitKeeper feud. It was written initially by Linus Torvalds, apparently during quite a lull in Linux development. It is for obvious reasons a very Linux focused tool, and I'd heard that performance is poor on Windows. I was not optimistic about it being usable on Windows.

Installation actually went very smoothly. <a href="http://code.google.com/p/msysgit/">Git for Windows</a> is basically powered by MSYS, the same Unix tools port that accompanies the Windows GCC port called MinGW. The installer is neat and sets up everything for you. It even offers a set of shell extensions that provide a graphical interface. Note that I opted not to install this interface, and I have no idea what it's like. A friend tells me it's awful.

Once the installer is done, git is ready to go. It's added to PATH and you can start cloning things right off the bat. Command line usage is simple and straightforward, and there's even a 'config' option that lets you set things up nicely without having to figure out what config file you want and where it lives. It's still a bit annoying, but I like it a lot better than Mercurial. I've heard some people complain about git being composed of dozens of binaries, but I haven't seen this on either my Windows or Linux boxes. I suspect this is a complaint about old versions, where each git command was its own binary (git-commit, git-clone, git-svn, etc), but that's long since been retired. Most of the installed binaries are just the MSYS ports of core Unix programs like <tt>ls</tt>.

I was also thrilled with the git-svn integration. Unlike Mercurial, the support is built in and flat out works with no drama whatsoever. I didn't try committing back into the Subversion repository from git, but apparently there is fabulous two way support. It was simple enough to create a git repository but it can be time consuming, since git replays every single check-in from Subversion to itself. I tested on a small repository with only about 120 revisions, which took maybe two minutes.

This is where I have to admit I have another motive for choosing Git. My favorite VCS frontend comes in a version called <a href="http://www.syntevo.com/smartgit/index.html">SmartGit</a>. It's a stand-alone (not shell integrated) client that is free for non commercial use and works really well. It even handled SSH beautifully, which I'm thankful about. It's still beta technically, but I haven't noticed any problems.

Now the rough stuff. I already mentioned that Git for Windows comes with a GUI that is apparently not good. What I discovered is that getting git to authenticate from Windows is fairly awful. In Subversion, you actually configure users and passwords explicitly in a plain-text file. Git doesn't support anything of the sort; their 'git-daemon' server allows fully anonymous pulls and can be configured for anonymous-push only. Authentication is entirely dependent on the filesystem permissions and the users configured on the server (barring workarounds), which means that most of the time, authenticated Git transactions happen inside SSH sessions. If you want to do something else, it's complicated at best. Oh, and good luck with HTTP integration if you chose a web server other than Apache. I have to imagine running a Windows based git server is difficult.

Let me tell you about SSH on Windows. It can be unpleasant. Most people use PuTTY (which is very nice), and if you use a server with public key authentication, you'll end up using a program called Pageant that provides that service to various applications. Pageant doesn't use OpenSSH compatible keys, so you have to convert the keys over, and watch out because the current stable version of Pageant won't do RSA keys. Git in turn depends on a third program called Plink, which exists to help programs talk to Pageant, and it finds that program via the GIT_SSH environment variable. The long and short of it is that getting Git to log into a public key auth SSH server is quite painful. I discovered that SmartGit simply reads OpenSSH keys and connects without any complications, so I rely on it for transactions with our main server.

I am planning to transition over to git soon, because I think that the workflow of a DVCS is better overall. It's really clear, though, that these are raw tools compared to the much more established and stable Subversion. It's also a little more complicated to understand; whether you're using git, Mercurial, or something else it's valuable to read the free ebooks that explain how to work with them. There are all kinds of quirks in these tools. Git, for example, uses a 'staging area' that clones your files for commit, and if you're not careful you can wind up committing older versions of your files than what's on disk. I don't know why -- seems like the opposite extreme from Mercurial. 

It's because of these types of issues that I favor choosing the version control system with the most momentum behind it. Git and Mercurial aren't the only two DVCS out there; Bazaar, monotone, and many more are available. But these tools already have rough (and sharp!) edges, and by sticking to the popular ones you are likely to get the most community support. Both Git and Mercurial have full blown books dedicated to them that are available electronically for free. My advice is that you read them.
