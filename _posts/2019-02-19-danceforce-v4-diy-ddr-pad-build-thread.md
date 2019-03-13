---
layout: post
title: DanceForce V4 DIY DDR Pad Build Thread
date: 2019-02-19 15:18
author: ventspace
comments: true
categories: [Non-technical]
featured: true
---
If you're unfamiliar with my DanceForce work or the previous versions, please read the introduction of my <a href="https://ventspace.wordpress.com/2018/04/09/danceforce-v3-diy-dance-pad-controller/">V3 build post</a> for the rationale and advantages of this particular approach to a hard pad and what I'm going for. In short, the DF is a slimmer, lighter hardpad that can be more reliable and consistent than conventional designs due to its use of pressure sensitive sensors that are separated from the "click action" of the actual steps.

I'm now building the DanceForce V4 prototype. V4 is simpler, easier to build, requires less parts, and is cheaper. Traditionally I build and design these pads, make a bunch of tweaks, and play on them for a good while. Then I begin working on the draft of the instructional write-up, and eventually publish the full how-to guide. If I followed that timeline again, this V4 guide would appear in <em>*checks notes*</em> summer 2020. Let's not do that. I began work this past weekend, so I'm just going to post a stream of photos and exactly what I'm doing as I go.

Excluding pad graphics and a few incidentals, this pad costs about $160 to put together.

<strong>Current Status: </strong>Core pad is done but top hasn't been installed and control board hasn't been assembled. These are not changed from V3.
<h2>Building the Base</h2>
<img src="https://i.imgur.com/O9s3tAZ.jpg" alt="" /> Basic layout sketch of the initial cut pad.[/caption]

<img src="https://i.imgur.com/IqSQ6Er.jpg" alt="" /> Note: the dimensions in this photo are slightly wrong and I had to go back and fix it. Always triple check your measurements before cutting and gluing![/caption]

The base layer is 1/2" plywood cut to 34" x 33". The extra inch on top will be useful for wiring. I've marked off the steps in pencil, and then begun adding the spacer layer. <del>I'm using 1/8" hardboard this time around, for shallower steps than in the past.</del> The bottom panels are 10.25" square, the top are 10.25" x 5.25". The upper panels are sized to leave space for Start/Select buttons. Next step is beginning to lay out the contacts. I'm using <a href="https://www.amazon.com/gp/product/B01L16U5CY/ref=ppx_yo_dt_b_asin_title_o05__o00_s01?ie=UTF8&amp;psc=1">3" copper tape</a> today, but 4" is probably even better because it's less work and barely costs any more. <strong>NOTE: </strong>Hardboard is completely flush, no click action at all. Use 1/4" MDF for the spacer panels instead, and construction paper over the sensors to fine tune if needed.

<img src="https://i.imgur.com/JHh4lDg.jpg" />I've added the hardboard spacers around the Start and Select buttons. Note that these go on AFTER the copper tape, which runs underneath it. Here's a detail shot of what you'll end up with:

<img src="https://i.imgur.com/NdM0tJV.jpg" />

Finally, all of the contacts get connected together with a plus to serve as the common contact for the step sensors.

<img src="https://i.imgur.com/OYU3VDO.jpg" />That concludes the base layer.
<h2>Sensor Construction</h2>
Start by building the top contact. Cut four 10.5" squares of Lexan, and cover one side in copper tape.

<img src="https://i.imgur.com/YYPrbHF.jpg" />

Add a little strip around to the top side to serve as our connection point for later.

<img src="https://i.imgur.com/afQtoGS.jpg" alt="" /> It's important here to place the contact strip off center. You don't want it touching the extension strip on the common contact. I also clip the corners to leave space between steps.[/caption]

Place an 11" square of Velostat over the bottom of the contact. It does not need to cover it completely.

<img src="https://i.imgur.com/5KmyE2r.jpg" />Then the top contact goes over it. The top contact MUST be insulated by Velostat on every edge or the step will not work. That's why we cut it a little small. I've moved to <a href="https://www.ebay.com/itm/Velostat-Linqstat-Pressure-Sensitive-Conductive-Film-60-X-11-6-mil-thick/202290579286">6 mil Velostat</a> in the V4 design due to the higher sensitivity of pure copper contacts.

<img src="https://i.imgur.com/XlFz9n0.jpg" />Finally, duct tape secures the sensor in place. I've done a couple experiments now and it appears that too much duct tape is a bad idea. This is a pressure sensor and excessive tape applies so much pressure that there isn't enough range left to reliably detect steps.

<img src="https://i.imgur.com/bPrJpQi.jpg?1" alt="" /> The clipped corners leave space for hardboard strips that will fill the space between diagonal steps.[/caption]

<img src="https://i.imgur.com/WOADlva.jpg" alt="" /> Four assembled sensors. It's a good idea to test them with a multimeter at this point, while the duct tape still isn't that strongly bonded. You're looking for 70+ ohms at rest, and sub-10 with foot pressure.[/caption]

I also add some corner boundaries at this stage. These are hardboard strips of 1.75" x 0.5" and they are important to have good corner separation of the steps. The gap is important, wiring is going to run through there.
<h2><img src="https://i.imgur.com/kYZf49l.jpg" /></h2>
<h2>Electrical</h2>
Get ready to break out the soldering iron - but we have some prep work to do first. Take a look at the edges where your top contacts are - is copper peeking out past the Velostat?

<img src="https://i.imgur.com/c8O6RXF.jpg" />We don't want this. It will short if we try to take the contact over this section. A little strip of electrical or duct tape will insulate the boundary.

<img src="https://i.imgur.com/ba1Zwz1.jpg" />That's better. Now I'm going to build a solder pad from two layers of copper tape.

[caption id="" align="alignnone" width="2048"]<img src="https://i.imgur.com/zymWBu0.jpg" alt="" width="2048" height="1536" /> This solder pad I've laid down does not connect to the top contact of the step yet. This way if the step needs to come out, the soldered wire can stay where it is.[/caption]

Now to solder some wires. It's important to leave lots of extra length when cutting the wire, I've been screwed multiple times by not having enough spare lead.

[caption id="" align="alignnone" width="2048"]<img src="https://i.imgur.com/8BxoBou.jpg" alt="" width="2048" height="1536" /> Be judicious with the heat. The copper tape solders decently enough but it's not going to tolerate the iron for an extended period.[/caption]

Finally, one more layer of copper tape will link the top contact to the solder pad and shield it all in one go.

<img src="https://i.imgur.com/d4A8pEY.jpg" />And now all four arrows wired up:

<img src="https://i.imgur.com/N1iNIyi.jpg" />

I'll finish up Start and Select later. For now, we really need to neaten up those wires. Find a hot glue gun, route the wires nicely up through the top of the pad, and glue them in place.

<img src="https://i.imgur.com/4ZmwWOu.jpg" />

<img src="https://i.imgur.com/9mXC8DE.jpg" />

And with that, the internal construction of the pad is complete.
