---
layout: post
title: Lua and LuaBind Binaries for VS 2010
date: 2010-09-19 10:40
author: ventspace
comments: true
categories: [lua, luabind, Software Engineering]
---
I've been working with Lua recently, and apparently neither Lua nor LuaBind come in binary distributions. They're also kind of a pain to build, so I thought I'd go ahead and post fully prepped binaries, built by and for VC 2010.
<a href="http://slimdx.org/promit/lua-5.1-vc2010.zip">Lua 5.1</a>
<a href="http://slimdx.org/promit/luabind-0.9-vc2010.zip">LuaBind 0.9</a>

A few things to be aware of:
<ul>
	<li>These are just built 32 bit binaries and nothing else. You will still need project sources for the headers.</li>
	<li>Lua is built as a release mode only DLL.</li>
	<li>LuaBind comes in debug and release variants as a static library.</li>
	<li>These are built using Multithreaded DLL and Multithreaded Debug DLL. You'll get  link errors if your project is statically linking to the CRT.</li>
</ul>


