---
layout: post
title: Quick tip: Retina mode in iOS OpenGL rendering is not all-or-nothing
date: 2014-10-14 15:50
author: ventspace
comments: true
categories: [Graphics]
---
Some of you are probably working on Retina support and performance for your OpenGL based game for iOS devices. If you're like us, you're probably finding that a few of the devices (*cough* iPad 3) don't quiiite have the GPU horsepower to drive your fancy graphics at retina resolutions. So now you're stuck with 1x and 4x MSAA, which performs decently well but frankly looks kind of bad. It's a drastic step down in visual fidelity, especially with all the alpha blend stuff that doesn't antialias. (Text!) Well it turns out you don't have to choose such a drastic step. Here's the typical enable-retina code you'll find on StackOverflow or whatever:
<code>
if([[UIScreen mainScreen] respondsToSelector:@selector(scale)] &amp;&amp; [[UIScreen mainScreen] scale] == 2)
{
self.contentScaleFactor = 2.0;
eaglLayer.contentsScale = 2.0;
}</code>
<code>
//some GL setup stuff
...

//get the correct backing framebuffer size
int fbWidth, fbHeight;
glGetRenderbufferParameteriv(GL_RENDERBUFFER, GL_RENDERBUFFER_WIDTH, &amp;fbWidth);
glGetRenderbufferParameteriv(GL_RENDERBUFFER, GL_RENDERBUFFER_HEIGHT, &amp;fbHeight);</code>

The respondsToSelector bit is pretty token nowadays - what was that, iOS 3? But there's not much to it. Is the screen a 2x scaled screen? Great, set our view to 2x scale also. Boom, retina. Then we ask the GL runtime what we are running at, and set everything up from there. The trouble is it's a very drastic increase in resolution, and many of the early retina devices don't have the GPU horsepower to really do nice rendering. The pleasant surprise is, the scale doesn't have to be 2.0. Running just a tiny bit short on fill?
<code>
if([[UIScreen mainScreen] respondsToSelector:@selector(scale)] &amp;&amp; [[UIScreen mainScreen] scale] == 2)
{
self.contentScaleFactor = 1.8;
eaglLayer.contentsScale = 1.8;
}
</code>
Now once you create the render buffers for your game, they'll appear at 1.8x resolution in each each direction, which is very slightly softer than 2.0 but much, much crisper than 1.0. I waited until after I Am Dolphin cleared the Apple App Store approval process, to make sure that they wouldn't red flag this usage. Now that it's out, I feel fairly comfortable sharing it. This can also be layered with multisampling (which I'm also doing) to fine tune the look of poly edges that would otherwise give away the trick. I use this technique to get high resolution, high quality sharp rendering at 60 fps across the entire range of Apple devices, from the lowly iPhone 4S, iPod 5, and iPad 3 on up.
