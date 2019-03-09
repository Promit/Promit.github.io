---
layout: post
title: Game Code Build Times: RAID 0, SSD, or both for the ultimate in speed?
date: 2014-09-16 19:08
author: ventspace
comments: true
categories: [Non-technical]
---
I've been in the process of building and testing a new machine using Intel's new X99 platform. This platform, combined with the new Haswell-E series of CPUs, is the new high end of what Intel is offering in the consumer space. One of the pain points for developers is build time. For our part, we're building in the general vicinity of 400K LOC of C++ code, some of which is fairly complex -- it uses standard library and boost headers, as well as some custom template stuff that is not simple to compile. The worst case is my five year old home machine, an i5-750 compiling to a single magnetic drive, which turns in a six minute full rebuild time. Certainly not the biggest project ever, but a pretty good testbed and real production code.

I wanted to find out what storage system layout would provide the best results. Traditionally game developers used RAID 0 magnetic arrays for development, but large capacity SSDs have now become common and inexpensive enough to entertain seriously for development use. I tested builds on three different volumes:
<ul>
	<li>A single Samsung 850 Pro 512 GB (boot)</li>
	<li>A RAID 0 of two Crucial MX100 512 GB</li>
	<li>A RAID 0 of three WD Black 4 TB (7200 rpm)</li>
</ul>
Both RAID setups were blank. The CPU is an i7-5930k hex-core (12 threads) and I've got 32 GB of memory on board. Current pricing for all of these storage configurations is broadly similar. Now then, the results. Will the Samsung drive justify its high price tag? Will the massive bandwidth of two striped SSDs scream past the competitors? Can the huge magnetic drives really compete with the pinnacle of solid state technology? Who will win?

Drumroll...

They're all the same.

All three configurations run my test build in roughly 45 seconds, the differences between them being largely negligible. In fact it's the WD Blacks that posted the fastest time at 42s. The obvious takeaway is that all of these setups are past the threshold where something else is the bottleneck. That something in this case is the CPU, and more specifically the overall hardware thread count. Overclocking the CPU from 3.5 to 4.5 did nothing to help. I've heard of some studios outfitting their engineers with dual Xeon setups, and it's not looking so crazy to do so when employee time is on the line. (The potential downside is that the machine starts to stray significantly from what the game will actually run on.) Given the results, and the sizes of modern game projects, I'd recommend using an inexpensive 500 GB SSD for a boot drive (Crucial MX100, Sandisk Ultra II, 840 EVO), and stocking up on the WD Blacks for data. Case closed.

But... as long as we're here, why don't we take a look at what these drives are benchmarking at? The 850 Pro is a monster of a drive. Those striped MX100s might be the real heros though; ATTO shows them flirting with a full gigabyte per second of sequential transfer. Here are the raw CrystalDiskMark numbers for all three:

Samsung 850 Pro:
<blockquote>Sequential Read : 520.557 MB/s
Sequential Write : 489.836 MB/s
Random Read 512KB : 407.993 MB/s
Random Write 512KB : 465.648 MB/s
Random Read 4KB (QD=1) : 24.216 MB/s [ 5912.1 IOPS]
Random Write 4KB (QD=1) : 71.216 MB/s [ 17386.7 IOPS]
Random Read 4KB (QD=32) : 398.378 MB/s [ 97260.3 IOPS]
Random Write 4KB (QD=32) : 331.571 MB/s [ 80950.0 IOPS]</blockquote>
2x Crucial MX100 in RAID 0:
<blockquote>Sequential Read : 898.908 MB/s
Sequential Write : 905.506 MB/s
Random Read 512KB : 695.787 MB/s
Random Write 512KB : 854.666 MB/s
Random Read 4KB (QD=1) : 26.271 MB/s [ 6413.8 IOPS]
Random Write 4KB (QD=1) : 110.554 MB/s [ 26990.8 IOPS]
Random Read 4KB (QD=32) : 430.077 MB/s [104999.3 IOPS]
Random Write 4KB (QD=32) : 413.606 MB/s [100978.0 IOPS]</blockquote>
3x WD Black 4TB in RAID 0:
<blockquote>Sequential Read : 530.522 MB/s
Sequential Write : 494.534 MB/s
Random Read 512KB : 61.752 MB/s
Random Write 512KB : 162.619 MB/s
Random Read 4KB (QD=1) : 0.724 MB/s [ 176.7 IOPS]
Random Write 4KB (QD=1) : 4.461 MB/s [ 1089.1 IOPS]
Random Read 4KB (QD=32) : 5.090 MB/s [ 1242.8 IOPS]
Random Write 4KB (QD=32) : 5.307 MB/s [ 1295.6 IOPS]</blockquote>
I don't claim that these numbers are reliable or representative. I am only posting them to provide a general sense of the performance characteristics involved in each choice. The SSDs decimate the magnetic drive setup for random ops, though the 512 KB values are respectable. I had expected the 4K random read, for which SSDs are known, to have a significant impact on build time, but that clearly isn't the case. The WDs are able to dispatch 177 of those per second; despite being 33x slower than the 850 Pro, this is still significantly faster than the compiler can keep up with. Even in the best case scenarios, a C++ compiler won't be able to clear out more than a couple dozen files a second.
