---
layout: post
title: C++-JSON Serialization 
date: 2012-10-08 14:40
author: ventspace
comments: true
categories: [Software Engineering]
---
I've decided to share some code today, just because I'm such a nice guy. Those of you who enjoy the more perverse ways to apply C++ tricks will enjoy this. Those who prefer simpler, more primitive approaches (that's not a bad thing) may not appreciate this creation as much. What I've got here is a utility class that makes it fairly straightforward to serialize C++ objects to and from JSON using the generally decent JsonCpp library. Hierarchies are properly saved and loaded with no real effort. It works well for us, and probably has plenty of limitations too. Maybe some of you out there will find it useful. It seems to be difficult to find decent serialization code that isn't also somehow awful to use.

This lives in a single file, but the bad news is it takes boost dependencies in order to get type traits. I think everything I'm using from boost is added to C++ core as of TR1, but I haven't checked. It also depends on JsonCpp, but changing it over to use other JSON, XML, binary, etc libraries shouldn't be terribly difficult. I don't know how this compares to other serialization libraries, but boost::serialization sounded like a train-wreck so I wrote my own.

Let's cover usage first. Generally speaking, you'll simply add a member function to a structure that declares the members to be serialized (free functions are allowed too). Each declaration is a string name for the value, and the variable to be serialized under that value. There's a few macros to combine those via preprocessor. Values can also be read-only or write-only serialized. The serializer is able to traverse vectors and structures, and will produce nicely structured JSON. Here's a sample:
[sourcecode language="cpp"]void Serialize(Vector3D&amp; vec, JsonSerializer&amp; s)
{
	//each Vector3D is written as an array
	s.Serialize(0, vec.x);
	s.Serialize(1, vec.y);
	s.Serialize(2, vec.z);
}

struct PathSave {

	bool LeftWall;
	bool RightWall;
	float LeftWallHeight;
	float RightWallHeight;

	vector&lt;Vector3D&gt;	c_Center,
				c_Left,
				c_Right;

	vector&lt;int&gt; hitPillarSingle_type;
	vector&lt;struct PCC&gt; hitPillarSingle_pcc;
	vector&lt;Vector3D&gt; hitPillarSingle_hh;

	int pathPointDensity;
	int pillarNum;
	
	void Serialize(JsonSerializer&amp; s)
	{
		s.SerializeNVP(LeftWall);
		s.SerializeNVP(RightWall);
		s.SerializeNVP(LeftWallHeight);
		s.SerializeNVP(RightWallHeight);
		
		s.Serialize(&quot;Center&quot;, c_Center);
		s.Serialize(&quot;Left&quot;, c_Left);
		s.Serialize(&quot;Right&quot;, c_Right);
		
		s.SerializeNVP(hitPillarSingle_pcc);
		s.SerializeNVP(hitPillarSingle_hh);
		s.SerializeNVP(hitPillarSingle_type);
		
		s.WriteOnly(NVP(pathPointDensity));
		s.ReadOnly(NVP(pillarNum));
	}
};

void SaveToFile(PathSave&amp; path)
{
	JsonSerializer s(true);
	path.Serialize(s);
	std::string styled = s.JsonValue.toStyledString();
	printf(&quot;Saved data:\n%s\n&quot;, styled.c_str());
}

bool LoadFromFile(const char* filename, PathSave&amp; path)
{
	std::string levelJson;
	bool result = PlatformHelp::ReadDocument(filename, levelJson);
	if(!result)
		return false;

	JsonSerializer s(false);
	Json::Reader jsonReader;
	bool parse = jsonReader.parse(levelJson, s.JsonValue);
	if(!parse)
		return false;
	
	path.Serialize(s);
	return true;
}
[/sourcecode]
And that will generally produce something that looks like this:
[sourcecode]
{
    &quot;LeftWall&quot; : true,
    &quot;LeftWallHeight&quot; : 4.50,
    &quot;RightWall&quot; : true,
    &quot;RightWallHeight&quot; : 4.50,
    &quot;Center&quot; : [
    [ 0.05266714096069336, 0.0, -15.13085746765137 ],
    [ 0.1941599696874619, 0.0, 1.553306341171265 ],
    [ 0.5984783172607422, 0.0, 50.54330444335938 ]
    ],
    &quot;Left&quot; : [
    [ -10.44694328308105, 0.0, -15.04044914245605 ],
    [ -15.55506420135498, 0.0, 1.709598302841187 ],
    [ -8.466680526733398, 2.896430828513985e-07, 42.68054962158203 ]
    ],
    &quot;Right&quot; : [
    [ 10.55227851867676, 0.0, -15.22126579284668 ],
    [ 15.94338321685791, 0.0, 1.397014379501343 ],
    [ 9.663637161254883, -1.829234150818593e-07, 58.40605926513672 ]
    ],
    &quot;hitPillarSingle_hh&quot; : null,
    &quot;hitPillarSingle_pcc&quot; : null,
    &quot;hitPillarSingle_type&quot; : null,
    &quot;pathPointDensity&quot; : 24,
    &quot;pillarNum&quot; : 0,
}
[/sourcecode]
Now I happen to think that's fairly tidy, as far as C++ serialization goes. Symmetry is maintained between read and write steps, and there's very little in the way of syntax magic. I do have a few macros in there (the stuff that says NVP), but they're optional and I find that they clean things up. Now shield your eyes, because here is the actual implementation.
[sourcecode lang="cpp"]
/*
 * Copyright (c) 2011-2012 Promit Roy
 * 
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the &quot;Software&quot;), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 * 
 * The above copyright notice and this permission notice shall be included in
 * all copies or substantial portions of the Software.
 * 
 * THE SOFTWARE IS PROVIDED &quot;AS IS&quot;, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 * THE SOFTWARE.
 */

#ifndef JSONSERIALIZER_H
#define JSONSERIALIZER_H

#include &lt;json/json.hpp&gt;
#include &lt;boost/utility.hpp&gt;
#include &lt;boost/type_traits.hpp&gt;
#include &lt;string&gt;

class JsonSerializer
{
private:
	//SFINAE garbage to detect whether a type has a Serialize member
	typedef char SerializeNotFound;
	struct SerializeFound { char x[2]; };
	struct SerializeFoundStatic { char x[3]; };
	
	template&lt;typename T, void (T::*)(JsonSerializer&amp;)&gt;
	struct SerializeTester { };
	template&lt;typename T, void(*)(JsonSerializer&amp;)&gt;
	struct SerializeTesterStatic { };
	template&lt;typename T&gt;
	static SerializeFound SerializeTest(SerializeTester&lt;T, &amp;T::Serialize&gt;*);
	template&lt;typename T&gt;
	static SerializeFoundStatic SerializeTest(SerializeTesterStatic&lt;T, &amp;T::Serialize&gt;*);
	template&lt;typename T&gt;
	static SerializeNotFound SerializeTest(...);
	
	template&lt;typename T&gt;
	struct HasSerialize
	{
		static const bool value = sizeof(SerializeTest&lt;T&gt;(0)) == sizeof(SerializeFound);
	};
	
	//Serialize using a free function defined for the type (default fallback)
	template&lt;typename TValue&gt;
	void SerializeImpl(TValue&amp; value,
						typename boost::disable_if&lt;HasSerialize&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		//prototype for the serialize free function, so we will get a link error if it's missing
		//this way we don't need a header with all the serialize functions for misc types (eg math)
		void Serialize(TValue&amp;, JsonSerializer&amp;);
		
		Serialize(value, *this);
	}

	//Serialize using a member function Serialize(JsonSerializer&amp;)
	template&lt;typename TValue&gt;
	void SerializeImpl(TValue&amp; value, typename boost::enable_if&lt;HasSerialize&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		value.Serialize(*this);
	}
	
public:
	JsonSerializer(bool isWriter)
	: IsWriter(isWriter)
	{ }
	
	template&lt;typename TKey, typename TValue&gt;
	void Serialize(TKey key, TValue&amp; value, typename boost::enable_if&lt;boost::is_class&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		JsonSerializer subVal(IsWriter);
		if(!IsWriter)
			subVal.JsonValue = JsonValue[key];
		
		subVal.SerializeImpl(value);
		
		if(IsWriter)
			JsonValue[key] = subVal.JsonValue;
	}
		
	//Serialize a string value
	template&lt;typename TKey&gt;
	void Serialize(TKey key, std::string&amp; value)
	{
		if(IsWriter)
			Write(key, value);
		else
			Read(key, value);
	}
	
	//Serialize a non class type directly using JsonCpp
	template&lt;typename TKey, typename TValue&gt;
	void Serialize(TKey key, TValue&amp; value, typename boost::enable_if&lt;boost::is_fundamental&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		if(IsWriter)
			Write(key, value);
		else
			Read(key, value);
	}
	
	//Serialize an enum type to JsonCpp 
	template&lt;typename TKey, typename TEnum&gt;
	void Serialize(TKey key, TEnum&amp; value, typename boost::enable_if&lt;boost::is_enum&lt;TEnum&gt; &gt;::type* dummy = 0)
	{
		int ival = (int) value;
		if(IsWriter)
		{
			Write(key, ival);
		}
		else
		{
			Read(key, ival);
			value = (TEnum) ival;
		}
	}
	
	//Serialize only when writing (saving), useful for r-values
	template&lt;typename TKey, typename TValue&gt;
	void WriteOnly(TKey key, TValue value, typename boost::enable_if&lt;boost::is_fundamental&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		if(IsWriter)
			Write(key, value);
	}
	
	//Serialize a series of items by start and end iterators
	template&lt;typename TKey, typename TItor&gt;
	void WriteOnly(TKey key, TItor first, TItor last)
	{
		if(!IsWriter)
			return;
		
		JsonSerializer subVal(IsWriter);
		int index = 0;
		for(TItor it = first; it != last; ++it)
		{
			subVal.Serialize(index, *it);
			++index;
		}
		JsonValue[key] = subVal.JsonValue;
	}
	
	template&lt;typename TKey, typename TValue&gt;
	void ReadOnly(TKey key, TValue&amp; value, typename boost::enable_if&lt;boost::is_fundamental&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		if(!IsWriter)
			Read(key, value);
	}

	template&lt;typename TValue&gt;
	void ReadOnly(std::vector&lt;TValue&gt;&amp; vec)
	{
		if(IsWriter)
			return;
		if(!JsonValue.isArray())
			return;
		
		vec.clear();
		vec.reserve(vec.size() + JsonValue.size());
		for(int i = 0; i &lt; JsonValue.size(); ++i)
		{
			TValue val;
			Serialize(i, val);
			vec.push_back(val);
		}
	}
	
	template&lt;typename TKey, typename TValue&gt;
	void Serialize(TKey key, std::vector&lt;TValue&gt;&amp; vec)
	{
		if(IsWriter)
		{
			WriteOnly(key, vec.begin(), vec.end());
		}
		else
		{
			JsonSerializer subVal(IsWriter);
			subVal.JsonValue = JsonValue[key];
			subVal.ReadOnly(vec);
		}
	}
	
	//Append a Json::Value directly
	template&lt;typename TKey&gt;
	void WriteOnly(TKey key, const Json::Value&amp; value)
	{
		Write(key, value);
	}
	
	//Forward a pointer
	template&lt;typename TKey, typename TValue&gt;
	void Serialize(TKey key, TValue* value, typename boost::disable_if&lt;boost::is_fundamental&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		Serialize(key, *value);
	}
	
	template&lt;typename TKey, typename TValue&gt;
	void WriteOnly(TKey key, TValue* value, typename boost::disable_if&lt;boost::is_fundamental&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		Serialize(key, *value);
	}
	
	template&lt;typename TKey, typename TValue&gt;
	void ReadOnly(TKey key, TValue* value, typename boost::disable_if&lt;boost::is_fundamental&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		ReadOnly(key, *value);
	}
	
	//Shorthand operator to serialize
	template&lt;typename TKey, typename TValue&gt;
	void operator()(TKey key, TValue&amp; value)
	{
		Serialize(key, value);
	}
	
	Json::Value JsonValue;
	bool IsWriter;
	
private:
	template&lt;typename TKey, typename TValue&gt;
	void Write(TKey key, TValue value)
	{
		JsonValue[key] = value;
	}
				  
	template&lt;typename TKey, typename TValue&gt;
	void Read(TKey key, TValue&amp; value, typename boost::enable_if&lt;boost::is_arithmetic&lt;TValue&gt; &gt;::type* dummy = 0)
	{
		int ival = JsonValue[key].asInt();
		value = (TValue) ival;
	}
	
	template&lt;typename TKey&gt;
	void Read(TKey key, bool&amp; value)
	{
		value = JsonValue[key].asBool();
	}
	
	template&lt;typename TKey&gt;
	void Read(TKey key, int&amp; value)
	{
		value = JsonValue[key].asInt();
	}
	
	template&lt;typename TKey&gt;
	void Read(TKey key, unsigned int&amp; value)
	{
		value = JsonValue[key].asUInt();
	}
	
	template&lt;typename TKey&gt;
	void Read(TKey key, float&amp; value)
	{
		value = JsonValue[key].asFloat();
	}
	
	template&lt;typename TKey&gt;
	void Read(TKey key, double&amp; value)
	{
		value = JsonValue[key].asDouble();
	}
	
	template&lt;typename TKey&gt;
	void Read(TKey key, std::string&amp; value)
	{
		value = JsonValue[key].asString();
	}
};

//&quot;name value pair&quot;, derived from boost::serialization terminology
#define NVP(name) #name, name
#define SerializeNVP(name) Serialize(NVP(name))

#endif
[/sourcecode]
Now that's not so bad, is it? A bit under three hundred lines of type traits and template games and we're ready to get on with our lives. A lot of the code is just fussing about what type it's being applied to and drilling down to the correct read or write function. The SFINAE based block at the top of the class is used to locate the correct Serialize function for any given type, which can be an instance member function, static member function, or free function.

There is your free C++ to JSON serializer utility class for the day, complete with ultra permissive license. Enjoy.
