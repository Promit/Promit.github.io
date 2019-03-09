---
layout: post
title: How to Serialize Interfaces in .NET
date: 2010-02-20 17:32
author: ventspace
comments: true
categories: [.net, csharp, Software Engineering]
---
I'm working on some final touches for SlimTune's next version, and one of them involves persisting the launcher settings between application runs.  Launching is handled by an interface ILauncher, which can be set to any number of things via a reflected list of inherited types. A PropertyGrid is used to configure the settings, and all the underlying code ever sees is the interface. SlimTune's a plugin based C# app, and this is all pretty standard.

When it came to persisting this data across sessions, I figured it'd be no big deal -- I'll just serialize the object out to isolated storage, and deserialize it again when I need it. There's one hang-up, though. Serializers (or at least XmlSerializer) can't handle interfaces! Worse still, the so-called solutions I found <a href="http://social.msdn.microsoft.com/Forums/en-US/asmxandxml/thread/bac96f79-82cd-4fef-a748-2a85370a8510/">online</a> <a href="http://jachman.wordpress.com/2009/04/08/how-to-serialize-interface-types-and-lists-using-the-xmlserializer/">were</a> <a href="http://bytes.com/topic/net/answers/172408-problem-serialize-property-return-interface">utterly</a> <a href="http://geekswithblogs.net/SoftwareDoneRight/archive/2008/01/16/how-to-serialize-an-interface-using-the-xmlserializer.aspx">ludicrous</a>. It turns out this is actually an incredibly easy problem to solve, and mainly involves stopping and thinking about what you're doing for about five seconds.

Alright, so we can't serialize an interface, but we can serialize any concrete type. Same goes for the deserialization process. The answer is to simple: store the concrete type with the serialized data.
[sourcecode language="csharp"]
//save the launcher configuration to isolated storage
var isoStore = IsolatedStorageFile.GetUserStoreForApplication();
using(var configFile = new IsolatedStorageFileStream(ConfigFile, FileMode.Create, FileAccess.Write, isoStore))
{
	var launcherType = m_launcher.GetType();
	//write the concrete type so we know what to deserialize
	string launcherTypeName = launcherType.AssemblyQualifiedName;
	var sw = new StreamWriter(configFile);
	sw.WriteLine(launcherTypeName);

	//write the object itself
	var serializer = new XmlSerializer(launcherType);
	serializer.Serialize(sw, m_launcher);
}
[/sourcecode]
We simply ask the interface to give us its real type, and record it to the file before serializing. Okay, so the result won't be a legal XML file, but how often is that actually a problem? Now the deserialize side of the equation:
[sourcecode language="csharp"]
//try and load a launcher configuration from isolated storage
var isoStore = IsolatedStorageFile.GetUserStoreForApplication();
using(var configFile = new IsolatedStorageFileStream(ConfigFile, FileMode.Open, FileAccess.Read, isoStore))
{
	//read the concrete type to deserialize
	var sr = new StreamReader(configFile);
	var launcherTypeName = sr.ReadLine();
	var launcherType = Type.GetType(launcherTypeName, true);

	//read the actual object
	XmlSerializer serializer = new XmlSerializer(launcherType);
	m_launcher = (ILauncher) serializer.Deserialize(sr);
}
[/sourcecode]
Reversing things, we first read the type that was written to file, and reconstruct the actual concrete type that goes with that string. Then we know exactly what to deserialize, and XmlSerializer is happy to oblige.

Now that wasn't so hard, was it?
