---
layout: post
title: "Windows Installer: Worse Than I Thought"
date: 2010-11-10 16:59
author: ventspace
comments: true
categories: [Software Engineering]
---
I hate Windows Installer. I <a href="http://ventspace.wordpress.com/2010/06/30/windows-installer-is-terrible/">really hate it</a>. So you have to understand, I was a little surprised to discover that it is actually <i>worse than I thought</i>.

I do not, generally speaking, have much of a problem with Microsoft or Windows. They've done a lot for me, I've done a few things for them, and it's been good. However, my feelings about Windows are continuing to degrade from "generally decent" to "least unpleasant". But that is a digression from the point at hand, which is a specific infuriating problem. Let me explain my machine's hard drive setup:
* 60 GB SSD. This is for OS and 'core' software.
* 400 GB magnetic drive for 'non-core' software.
* 1 TB drive for personal data.
I figured that 60 GB would surely be plenty for the OS and small programs. The Windows folder now accounts for a mind boggling 25.7 GB of usage on that drive. I was wondering why.
<a href="http://ventspace.files.wordpress.com/2010/11/drive-space.png"><img src="http://ventspace.files.wordpress.com/2010/11/drive-space.png" alt="" title="Drive Space" width="600" class="alignnone size-medium wp-image-710" /></a>
Our culprits are apparently Installer and WinSxS. I'll leave the latter for another time and dig into why Installer is so big.

As I've discussed in the past, Installer files (msi, msp) are essentially embedded database files that store everything that is required in order to install or uninstall a program. Notice the flip side of the coin here -- the only way to <i>uninstall</i> (or repair for that matter) is to save the installer package. And these packages always get saved in the same spot, regardless of where you actually installed the program.

The short version is that if you have a small C drive but run lots of software, you will eventually run out of space even if all the software lives elsewhere. At this point I'm left to throw up my hands and ask the same thing I've asked many times before. What idiot was in charge of designing Windows Installer? The engineering here is slipshod and frankly embarrassing.

On the bright side, the solution to this problem is actually relatively straightforward. There's nothing stopping you from moving the contents of Installer to another drive and creating a junction to fill in for it. Just like that, I can cross off 8 GB that definitely don't need to be on my two-dollar-per-gigabyte SSD. 
