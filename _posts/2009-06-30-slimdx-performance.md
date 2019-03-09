---
layout: post
title: SlimDX Performance
date: 2009-06-30 08:00
author: ventspace
comments: true
categories: [c++, Performance, SlimDX]
---
Whoo, I burnt through a lot of posts last week. I really ought to pace myself a bit more conservatively.
<hr>

I'm not equipped at the moment to undertake a <i>full</i> treatment of SlimDX's performance right now, as that's a rather touchy set of problems. However, I did want to provide a general overview of what kinds of things affect SlimDX performance and what you can expect from your own programs. Strictly from the library's point of view, performance is actually surprisingly good. It is <i>not</i> as good as unmanaged code -- and can never be -- but it is much better than you might think.

There are a few key sources of inefficiency in SlimDX, which are varied and complex. Some are avoidable, and some are not. Some are inherent to the process as a whole, and some are results of architectural decisions in building the library. The name "Slim" officially means that there's a very minimal barrier between you and DirectX. While this is basically true, there's still a lot details to be aware of.

The very process of exposing a native API to managed code is complex. Apart from converting the parameters themselves -- a process which SlimDX is exceedingly efficient at -- there is a lot of bookkeeping to do in making sure the call stacks stay correct, that permissions are handled properly, that exceptions are trapped safely, and so on. C++/CLI handles all of this for us behind the scenes, but there is a substantial cost for making ANY native call as a result. We have studied techniques for addressing those costs by allowing you to batch certain kinds of calls (eg SetRenderState), but nothing has yet shown itself to be clearly advantageous.

The most obvious candidate for performance issues is the math library. Floating point is well known for being performance critical in games (and certain other software) and that's why we have processor extensions like SSE to make it as fast as possible. The D3DX routines that Microsoft provides are heavily optimized, and take full advantage of processor extensions. Unfortunately, using these routines from managed code is very expensive, so we instead provide completely managed implementations. XNA takes the same approach, but MDX calls into D3DX. These managed functions will JIT into <i>scalar</i> SSE which, while not optimal, is still very fast. Rough benchmarks have shown that completely native D3DX code has about a 10% advantage over our implementation. In other words, if you spend 20% of your time just doing math, a native code version could be 2% faster in that respect. In order to get the best possible performance out of SlimDX -- or XNA, for that matter -- make sure to use the ref/out overloads of parameters. It won't get you Havok levels of math performance, but it's still extremely fast.

The vast majority of calls into the DirectX API itself return an HRESULT error code. In C++, you're free to check or ignore this as you please. MDX took the fairly aggressive step of checking every single return value and converting to an exception; SlimDX and XNA both follow essentially that same model. SlimDX is particularly powerful, because it allows you to trap specific failed results, to control what actually causes an exception, or to disable exceptions outright. There's also a LastError facility that stores a thread-local value for the last result code recorded by SlimDX. It's a lot of truly useful flexibility, but it comes with a cost. For the March 2009 release, we leaned out the error checking significantly, making it much, much cheaper to make short DirectX calls. For a successful result, you basically pay for a method call, a comparison, and a small write to thread-local storage. A failed result triggers a much slower path. In the end, we are occasionally slower than MDX, but usually not thanks to improvements in .NET 2.0 and our improved architecture overall. We're also nearly always faster than XNA on the CPU; XNA spends a lot of time doing thorough parameter validation.

Be wary of properties, especially ones that return complex value types. Many properties will cause a DirectX call on every invocation. If the return type is large, we're probably reading an entire struct back, and then copying it to your variable. The Description property on many objects, for example, causes a GetDesc call. If you check obj.Description.Width and obj.Description.Height, that is <i>two</i> calls to GetDesc and <i>two</i> full copy operations. Cache the return value if you're invoking DirectX a lot in performance sensitive code areas!

Anything in SlimDX that returns a ComObject derivative is typically translating from a native pointer to a managed object. In SlimDX, this invokes some internal machinery that will take a lock, do a table lookup, possibly insert to the table, and then construct an object if necessary. In the vast majority of cases this should never be an issue, but if you're getting lock contention on our object table there is potential for problems. We don't really have good data on this one either way, so it's probably safe to assume it's not a concern. The lock is only held for very short amounts of time.

Lastly, <b>cache all of your effect handles</b> when working with D3DX effects! Because these are string pointers internally, and D3DX doesn't support unicode for them, we have to allocate every time a handle is created. Passing a string instead of a cached effect handle will cause allocation, every call and every frame. The net effect on performance of passing strings directly is quite extreme, since it follows slow paths in both SlimDX and D3DX.
 
That's a somewhat high level overview, and I'm planning to add documentation that discusses SlimDX's performance characteristics in more detail. However, we've been completely silent on performance and I decided to at least rectify that in brief. If I had to summarize, I'd say that in the general case:

<ul>
<li>XNA and SlimDX are both faster than MDX, and sometimes <i>much</i> faster.</li>
<li>SlimDX is faster than XNA at specific tasks, but it's unlikely to make a noticeable difference for most people.</li>
<li>SlimDX is still slower than native code, probably on the order of 5%-10% with respect to DirectX calls. So if 50% of your time is spent calling SlimDX/DirectX (VERY high), you could be running as much as 5% faster in native code in that respect, but 3% is a more realistic estimate. For sane applications, even less.</li></ul>
