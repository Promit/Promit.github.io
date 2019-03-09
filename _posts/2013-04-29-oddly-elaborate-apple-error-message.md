---
layout: post
title: Oddly Elaborate Apple Error Message
date: 2013-04-29 15:51
author: ventspace
comments: true
categories: [Non-technical]
---
I just wanted to share this. Popped up today while initializing an NSDateComponents object.
<code>
components:fromDate:toDate:options:]: fromDate cannot be nil
I mean really, what do you think that operation is supposed to mean with a nil fromDate?
An exception has been avoided for now.
A few of these errors are going to be reported with this complaint, then further violations will simply silently do whatever random thing results from the nil.
Here is the backtrace where this occurred this time (some frames may be missing due to compiler optimizations):
</code>
So that was unexpected.
