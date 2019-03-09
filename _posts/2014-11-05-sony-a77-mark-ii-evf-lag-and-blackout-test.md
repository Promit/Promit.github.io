---
layout: post
title: "Sony A77 Mark II: EVF Lag and Blackout Test"
date: 2014-11-05 16:05
author: ventspace
comments: true
categories: [Non-technical]
---
I'm planning to review this camera properly at some point, but for the time being, I wanted to do a simple test of what the parameters of EVF lag and blackout are.

Let's talk about lag first. What do we mean? The A77 II uses an electronic viewfinder, which means that the viewfinder is a tiny LCD panel, showing a feed of what the imaging sensor currently sees. This view takes camera exposure and white balance into exposure, allowing you to get a feel for what the camera is actually going to record when the shutter fires. However, downloading and processing the sensor data, and then showing it on the LCD, takes time. This shutter firing needs to compensate for this lag; if you hit the shutter at the exact moment an event occurs on screen, the lag is how late you will actually fire the shutter as a result.

How do we test the lag? Well, the A77 II's rear screen shows exactly the same display as the viewfinder, presumably with very similar lag. So all we have to do is point the camera at an external timer, and photograph both the camera and the timer simultaneously. And so that's exactly what I did.
<a href="https://ventspace.files.wordpress.com/2014/11/p1030514-screen.jpg"><img class="aligncenter size-large wp-image-1087" src="https://ventspace.files.wordpress.com/2014/11/p1030514-screen.jpg?w=660" alt="P1030514-screen" width="660" height="880" /></a>
Note that I didn't test whether any particular camera settings affected the results. The settings are pretty close to defaults. "Live View Display" is set to "Setting Effect ON". These are the values I got, across 6 shots, in millseconds:
32, 16, 17, 34, 17, 33 = <strong>24.8 ms average</strong>
I discarded a few values due to illegible screen (mid transition), but you get the picture. The rear LCD, and my monitor, are running at a 60 Hz refresh rate, which means that a new value appears on screen every ~16.67 ms. The lag wobbles between one and two frames, but this is mostly due to the desynchronization of the two screen refresh intervals. It's not actually possible to measure any finer using this method, unfortunately. However the average value gives us a good ballpark value of effectively 25 ms. Consider that a typical computer LCD screen is already going to be in the 16ms range for lag, and TVs are frequently running in excess of 50ms. This is skirting the bottom of what the fastest humans (pro gamers etc) can detect. Sony's done a very admirable job of getting the lag under control here.

Next up: EVF blackout. What is it? Running the viewfinder is essentially a continuous video processing job for the camera, using the sensor feed. In order to take a photo, the video feed needs to be stopped, the sensor needs to be blanked, the exposure needs to be taken, the shutter needs to be closed, the image downloaded off the sensor into memory, then the shutter must open again and the video feed must be resumed. The view of the camera goes black during this entire process, which can take quite a long time. To test this, I simply took a video of the camera while clicking off a few shots (1/60 shutter) <em>in single shot mode</em>. Here's a GIFed version at 20 fps:
<img class="aligncenter size-full wp-image-1089" src="https://ventspace.files.wordpress.com/2014/11/p1030523.gif" alt="P1030523" width="480" height="270" />
By stepping through the video, I can see how long the screen is black. These are the numbers I got, counted in 60 Hz video frames:
17, 16, 16, 17, 16, 16 = <strong>272 ms average</strong>
The results here are very consistent; we'll call it a 0.27 second blackout time. For comparison, Canon claims that the mirror blackout on the Canon 7D is 0.055 seconds, so this represents a substantial difference between the two cameras. It also seems to be somewhat worse than my Panasonic GH4, another EVF based camera, although I haven't measured it. I think this is an area which Sony needs to do a bit more, and I would love to see a firmware update to try and get this down at least under 200 ms.

It's worth noting that the camera behaves differently in burst mode, going to the infamous "slideshow" effect. At either 8 or 12 fps settings, the screen shows the shot just taken rather than a live feed. This quantization makes "blackout time" slightly meaningless, but it can present a challenge when tracking with erratically moving subjects.
