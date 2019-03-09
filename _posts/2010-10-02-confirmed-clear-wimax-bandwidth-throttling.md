---
layout: post
title: "Confirmed: Clear WiMax Bandwidth Throttling"
date: 2010-10-02 12:44
author: ventspace
comments: true
categories: [4g, clear, Non-technical, throttling, wimax]
---
Earlier this week, <a href="http://www.engadget.com/2010/09/29/clearwire-throttling-at-home-wimax-users/">an Engadget post appeared</a> reporting that Clear WiMax (aka 4G wireless) throttles bandwidth for users who have used a 'substantial' amount of bandwidth. The exact amount isn't clear but 10GB appears as a common number. Once you trigger the throttle, you're stuck at 0.25 megabit for an indeterminate amount of time. As it turns out, I've signed up for Clear and am still within their trial period. If there's a cap, now would be the time to find it.

Some background: Clear is a WiMax service (formerly known as Clearwire), owned mostly by Sprint and with significant investment from Comcast. They allow you to sign up for 'unlimited' service at a fairly reasonable fee. Although they don't promise specific speeds, they've clocked in at 10+ megabits for home service with a good signal, and mobile on the go service that can do about 5-6 megabits as well. They've been pushing it very hard here in Baltimore, which was one of the very first markets to get it (Portland being the other). I liked the idea and decided to try it.

I tested for a bandwidth cap by starting to download fairly substantial but still reasonable amounts of data on the third day of my service. The data was mainly HTTP downloads from TechNet/MSDN, but also included some low level torrent activity overnight. I started the testing on Wednesday, Sept. 29 2010. Today morning is Saturday, Oct 2 and I've hit the cap. Speedtest.net reports about 0.24 megabit. As it turns out, Clear has a site that reports how much bandwidth you're using. Let's see how I did!
<a href="http://ventspace.files.wordpress.com/2010/10/clear-bandwidth.png"><img src="http://ventspace.files.wordpress.com/2010/10/clear-bandwidth.png?w=600" alt="" title="Clear Bandwidth" class="alignnone size-medium wp-image-667" /></a>
Not so hot. The total is just north of 18GB and the throttle has kicked in. I don't consider myself a heavy bandwidth user, and I think bandwidth caps (like Comcast's 250GB) are completely reasonable. However, 18GB is simply ridiculous for a service that advertises itself as 'unlimited', especially when Clear's customer service assured me earlier in the week that there is no throttling. This is a data level that would be easy to hit using video streaming services or digitally purchasing software (Steam!).

How does this compare to the Verizon ADSL service I already had? Pricing is the same at about $45 per month for home service, but Clear has me on a two year contract while Verizon is month-to-month. Speeds are about the same at about 6.5 megabits peak. I've never hit any kind of cap on Verizon's service, whereas I crashed into Clear's cap almost immediately.

I'll be canceling my Clear service first thing Monday morning. 

<b>UPDATE:</b> Take a look at this graph from <a href="http://techblog.netflix.com/2011/01/netflix-performance-on-top-isp-networks.html">Netflix</a>:
<img src="http://ventspace.files.wordpress.com/2010/10/isp_usa.png?w=600" />
See which ISP is on the bottom? Clearwire.
