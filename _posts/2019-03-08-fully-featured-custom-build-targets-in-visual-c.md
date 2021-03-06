---
layout: post
title: Fully Featured Custom Build Targets in Visual C++
date: 2019-03-08 16:39
author: ventspace
comments: true
categories: [Software Engineering]
tags: [c++, MSBuild, visual studio]
image: custombuildtool.png
summary: A few days ago I was setting up a new resource build pipeline for our games, and wanted to integrate the build directly in Visual Studio. The goal was to include a resource manifest file in the project, and have them be fed to my compiler as part of the normal VC project build.
---

<p>A few days ago I was setting up a new resource build pipeline for our games, and wanted to integrate the build directly in Visual Studio. The goal was to include a resource manifest file in the project, and have them be fed to my compiler as part of the normal VC project build. Often the starting point for this is a simple command line entered as a <a href="https://docs.microsoft.com/en-us/cpp/ide/specifying-build-events?view=vs-2017">Custom Build Event</a>, but those are basically just dumb commands that don't follow the project files at all. The next step up from there is configuring a <a href="https://docs.microsoft.com/en-us/cpp/ide/specifying-custom-build-tools?view=vs-2017">Custom Build Tool</a> on the files in question. This works well once you have it set up, but there are distinct drawbacks. Each file is configured completely separately, and there's no way to share configuration. Adding the file to the project doesn't do anything unless you go in and set several properties for the build tool. There has to be a better way.</p>

<!-- wp:image {"id":1695} -->
![]({{ site.post_images}}/custombuildtool.png)
*Setting all of these fields up gets old real quick.*
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>After asking around for that better way, I was pointed to Nathan Reed's excellent write-up on <a href="http://reedbeta.com/blog/custom-toolchain-with-msbuild/">Custom Targets</a> and toolchains in VS. By setting up this functionality, you can configure a project to automatically recognize certain file extensions, and execute a predefined build task command line on all of them with correct incremental builds. This build customization system works great, and is absolutely worth setting up if that's all you need! I followed those instructions and had my resource manifests all compiling nicely into the project - until I wanted to add an extra command line flag to just one file. It turns out that while the build customization targets are capable of a lot, the approach Nathan takes only takes you so far and effectively forces you to run the same command line for all of your custom build files.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1696} -->
![]({{ site.post_images}}/customtarget1.png)
*The file is now recognized as a "Resource Pack" and will build appropriately! But we have no options about how to build it and no ability to tweak the command line sent.*
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>With some help from Nathan and a lot of futzing around with modifications of the custom build targets included with VS, I've managed to do one better and integrate my resource builds fully into VS, with property pages and configurable command line. What follows is mostly just a stripped down copy of the masm (Microsoft Macro Assembler) build target, but should offer a good basis to work from. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1701} -->
![]({{ site.post_images}}/customtarget-final-1.png)
*Now we have property pages for our custom target, along with custom properties for the command line switches.*
<!-- /wp:image -->

<!-- wp:image {"id":1698} -->
![]({{ site.post_images}}/customtarget-cmdline.png)
*Command line display, including a box to insert additional options.*
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>For this type of custom build target, there are three files you will need: .props, .xml, and .targets. We'll look at them in that order. Generally all three files should have the same name but the appropriate extension. Each one has a slightly different purpose and expands on the previous file's contents. I'm not going to dwell too much on the particulars of MSBuild's elements and format, but focus on providing listings and overviews of what's going on.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The props file provides the basic properties that define our custom build task. </p>
<!-- /wp:paragraph -->

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
 
  <ItemDefinitionGroup>
    <ResourcePackTask>
      <!--Enter Defaults Here-->
      <ComputeHashes>false</ComputeHashes>
      <SuppressJson>false</SuppressJson>
      <BuildLog>false</BuildLog>
      <ManifestFileName>$(OutDir)Resources\%(Filename).pack</ManifestFileName>
      <AdditionalOptions></AdditionalOptions>
 
      <CommandLineTemplate>$(KataTools)KataBuild.exe [AllOptions] [AdditionalOptions] --manifest %(FullPath) $(OutDir)Resources</CommandLineTemplate>
    </ResourcePackTask>
  </ItemDefinitionGroup>
 
</Project>
```

<!-- wp:paragraph -->
<p>My task is called "ResourcePackTask" and you'll see that name recurring throughout the code. What I'm doing here is to define the properties that make up my ResourcePackTask, and give them default values. The properties can be anything you like; in my case they're just names representing the command line switches I want to provide as options. These are not necessarily GUI-visible options, as that will be configured later. Just think of it as a structure with a bunch of string values inside it, that we can reference later as needed. The key component in this file is the CommandLineTemplate, which makes up its own syntax for options that doesn't seem to appear anywhere else. [AllOptions] will inject the switches configured in the GUI, and [AdditionalOptions] will add the text from the Command Line window. It's otherwise normal MSBuild syntax and macros.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Next up is the .xml file. This file's main role is to configure the Visual Studio GUI appropriately to reflect your customization. Note that VS is a little touchy about when it reads this file, and you may need to restart the IDE for changes to be reflected. We'll start with this basic version that doesn't add any property sheets:</p>
<!-- /wp:paragraph -->

```xml
<?xml version="1.0" encoding="utf-8"?>
 
<ProjectSchemaDefinitions xmlns="http://schemas.microsoft.com/build/2009/properties" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" xmlns:sys="clr-namespace:System;assembly=mscorlib">
  <ItemType
   Name="ResourcePackTask"
   DisplayName="Resource Pack" />
  <ContentType
    Name="ResourcePackTask"
    DisplayName="Resource Pack"
    ItemType="ResourcePackTask" />
  <FileExtension Name=".pack" ContentType="ResourcePackTask" />
</ProjectSchemaDefinitions>
```

<!-- wp:paragraph -->
<p>So far we've told the IDE that any time it sees a file with the extension ".pack", it should automatically categorize that under "ResourcePackTask". (I'm unsure of the difference between ContentType and ItemType and also don't care.) This will put the necessary settings into place to run our builds, but it would also be nice to have some property sheets. They're called "Rules" in the XML file for some reason, and the syntax is straightforward once you have a reference:</p>
<!-- /wp:paragraph -->

```xml
<?xml version="1.0" encoding="utf-8"?>
 
<!-- This file tells the VS IDE what a resource pack file is and how to categorize it -->
 
<ProjectSchemaDefinitions xmlns="http://schemas.microsoft.com/build/2009/properties" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" xmlns:sys="clr-namespace:System;assembly=mscorlib">
  <Rule Name="ResourcePackTask"
        PageTemplate="tool"
        DisplayName="Resource Pack"
        SwitchPrefix=""
        Order="300">
 
    <Rule.Categories>
      <Category Name="General" DisplayName="General" />
      <Category
        Name="Command Line"
        Subtype="CommandLine">
        <Category.DisplayName>
          <sys:String>Command Line</sys:String>
        </Category.DisplayName>
      </Category>
    </Rule.Categories>
 
    <Rule.DataSource>
      <DataSource Persistence="ProjectFile" ItemType="ResourcePackTask" Label="" HasConfigurationCondition="true" />
    </Rule.DataSource>
 
    <StringProperty
      Name="Inputs"
      Category="Command Line"
      IsRequired="true">
      <StringProperty.DataSource>
        <DataSource
          Persistence="ProjectFile"
          ItemType="ResourcePackTask"
          SourceType="Item" />
      </StringProperty.DataSource>
    </StringProperty>
    <StringProperty
      Name="CommandLineTemplate"
      DisplayName="Command Line"
      Visible="False"
      IncludeInCommandLine="False" />
    <StringProperty
      Subtype="AdditionalOptions"
      Name="AdditionalOptions"
      Category="Command Line">
      <StringProperty.DisplayName>
        <sys:String>Additional Options</sys:String>
      </StringProperty.DisplayName>
      <StringProperty.Description>
        <sys:String>Additional Options</sys:String>
      </StringProperty.Description>
    </StringProperty>
    <BoolProperty Name="ComputeHashes"
                  DisplayName="Compute resource hashes"
                  Description="Specifies if the build should compute MurMur3 hashes of every resource file. (--compute-hashes)"
                  Category="General"
                  Switch="--compute-hashes">
    </BoolProperty>
 
    <BoolProperty Name="SuppressJson"
                  DisplayName="Suppress JSON output"
                  Description="Specifies if JSON diagnostic manifest output should be suppressed/disabled. (--no-json)"
                  Category="General"
                  Switch="--no-json">
    </BoolProperty>
 
    <BoolProperty Name="BuildLog"
                  DisplayName="Generate build log"
                  Description="Specifies if a build log file should be generated. (--build-log)"
                  Category="General"
                  Switch="--build-log">
    </BoolProperty>
 
  </Rule>
 
  <ItemType
   Name="ResourcePackTask"
   DisplayName="Resource Pack" />
  <ContentType
    Name="ResourcePackTask"
    DisplayName="Resource Pack"
    ItemType="ResourcePackTask" />
  <FileExtension Name=".pack" ContentType="ResourcePackTask" />
</ProjectSchemaDefinitions>
```

<!-- wp:paragraph -->
<p>Again I don't ask too many questions here about this thing, as it seems to like looking a certain way and I get tired of constantly reloading the IDE to see if it likes a particular variation of the format. The file configures the categories that should show in the properties pane, indicates that the properties should be saved in the project file, and then lists the actual properties to display. I'm using StringProperty and BoolProperty, but two others of interest are StringListProperty (which works like the C++ include directories property) and EnumProperty (which works like any number of multi-option settings). Here's a sample of the latter, pulled from the MASM.xml customization:</p>
<!-- /wp:paragraph -->

```xml
<EnumProperty
  Name="ErrorReporting"
  Category="Advanced"
  HelpUrl="https://msdn.microsoft.com/library/default.asp?url=/library/en-us/vcmasm/html/vclrfml.asp"
  DisplayName="Error Reporting"
  Description="Reports internal assembler errors to Microsoft.     (/errorReport:[method])">
  <EnumValue
    Name="0"
    DisplayName="Prompt to send report immediately (/errorReport:prompt)"
    Switch="/errorReport:prompt" />
  <EnumValue
    Name="1"
    DisplayName="Prompt to send report at the next logon (/errorReport:queue)"
    Switch="/errorReport:queue" />
  <EnumValue
    Name="2"
    DisplayName="Automatically send report (/errorReport:send)"
    Switch="/errorReport:send" />
  <EnumValue
    Name="3"
    DisplayName="Do not send report (/errorReport:none)"
    Switch="/errorReport:none" />
</EnumProperty>
```

<!-- wp:paragraph -->
<p>All of these include a handy Switch parameter, which will eventually get pasted into our command line. At this point the IDE now knows what files we want to categorize, how to categorize them, and what UI to attach to them. The last and most complex piece of the puzzle is to tell it what to <em>do</em> with the files, and that's where the .targets file comes in. I'm going to post this file in a few pieces and go over what each piece does.</p>
<!-- /wp:paragraph -->

```xml
<?xml version="1.0" encoding="utf-8"?>
 
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
 
  <ItemGroup>
    <PropertyPageSchema
      Include="$(MSBuildThisFileDirectory)$(MSBuildThisFileName).xml" />
    <AvailableItemName Include="ResourcePackTask">
      <Targets>_ResourcePackTask</Targets>
    </AvailableItemName>
  </ItemGroup>
  ```

<!-- wp:paragraph -->
<p>First, we declare that we want to attach property pages to this target, point the IDE to the xml file from before, and tell it what the name of the items is that we want property pages for. We also give it a Target name (_ResourcePackTask) for those items, which will be referenced again later.</p>
<!-- /wp:paragraph -->

```xml
<UsingTask
  TaskName="ResourcePackTask"
  TaskFactory="XamlTaskFactory"
  AssemblyName="Microsoft.Build.Tasks.v4.0, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a">
  <Task>$(MSBuildThisFileDirectory)$(MSBuildThisFileName).xml</Task>
</UsingTask>
```

<!-- wp:paragraph -->
<p>This is the weird part. In Nathan's write-up, he uses a CustomBuild element to run the outside tool, but CustomBuild doesn't have a way of getting the command line switches we set up. Instead we're going to ask the MSBuild engine to read the provided assembly and ask its XamlTaskFactory to <em>generate </em>our ResourcePackTask. That XamlTaskFactory compiles a new C# Task object <em>on the fly</em> by reflecting our definitions from the .xml file (and maybe the .props file). This seems like an insane way to design a build system to me, but what do I know? In any case that seems to be how all of the MS tasks are implemented out of the box, and we'll follow their lead verbatim. Let's move on.</p>
<!-- /wp:paragraph -->

```xml
<Target Name="_WriteResourcePackTaskTlogs"
      Condition="'@(ResourcePackTask)' != '' and '@(SelectedFiles)' == ''">
  <ItemGroup>
    <_ResourcePackTaskReadTlog Include="^%(ResourcePackTask.FullPath);%(ResourcePackTask.AdditionalDependencies)"
                   Condition="'%(ResourcePackTask.ExcludedFromBuild)' != 'true' and '%(ResourcePackTask.ManifestFileName)' != ''"/>
    <!-- This is the important line to configure correctly for tlogs -->
    <_ResourcePackTaskWriteTlog Include="^%(ResourcePackTask.FullPath);$([MSBuild]::NormalizePath('$(OutDir)Resources', '%(ResourcePackTask.ManifestFileName)'))"
                    Condition="'%(ResourcePackTask.ExcludedFromBuild)' != 'true' and '%(ResourcePackTask.ManifestFileName)' != ''"/>
  </ItemGroup>
 
  <WriteLinesToFile
    Condition="'@(_ResourcePackTaskReadTlog)' != ''"
    File="$(TLogLocation)ResourcePackTask.read.1u.tlog"
    Lines="@(_ResourcePackTaskReadTlog->MetaData('Identity')->ToUpperInvariant());"
    Overwrite="true"
    Encoding="Unicode"/>
  <WriteLinesToFile
    Condition="'@(_ResourcePackTaskWriteTlog)' != ''"
    File="$(TLogLocation)ResourcePackTask.write.1u.tlog"
    Lines="@(_ResourcePackTaskWriteTlog->MetaData('Identity')->ToUpperInvariant());"
    Overwrite="true"
    Encoding="Unicode"/>
 
  <ItemGroup>
    <_ResourcePackTaskReadTlog Remove="@(_ResourcePackTaskReadTlog)" />
    <_ResourcePackTaskWriteTlog Remove="@(_ResourcePackTaskWriteTlog)" />
  </ItemGroup>
</Target>
```

<!-- wp:paragraph -->
<p>MSBuild operates by executing targets based on a dependency tree. This next section configures a Target that will construct a pair of .tlog files which record the dependencies and outputs, and enable the VS incremental build tracker to function. Most of this seems to be boring boilerplate. The key piece is where [MSBuild]::NormalizePath appears. This little function call assembles the provided directory path and filename into a final path that will be recorded as the corresponding build output file for the input. I have a hard coded Resources path in here for now, which you'll need to replace with something meaningful. The build system will look for this exact filename when deciding whether or not a given input needs to be recompiled, and you can inspect what you're getting in the resulting tlog file. If incremental builds aren't working correctly, check that file and check what MSBuild is looking for in the Diagnostic level logs.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I should note at this point that the tlog target is optional, and that as written it only understands the direct source file and its direct output. In my case, it will see changes to the resource manifest file, and it will see if the output is missing. But it has no information about other files read by that compile process, so if I update a resource referenced by my manifest it won't trigger a recompile. Depending on what you're doing, it may be better to omit the tlog functionality and do your own incremental processing. Another possibility is writing a process that generates the proper tlog. </p>
<!-- /wp:paragraph -->

```xml
<Target
    Name="_ResourcePackTask"
    BeforeTargets="ClCompile"
    Condition="'@(ResourcePackTask)' != ''"
    Outputs="%(ResourcePackTask.ManifestFileName)"
    Inputs="%(ResourcePackTask.Identity)"
    DependsOnTargets="_WriteResourcePackTaskTlogs;_SelectedFiles"
    >
    <ItemGroup Condition="'@(SelectedFiles)' != ''">
      <ResourcePackTask Remove="@(ResourcePackTask)" Condition="'%(Identity)' != '@(SelectedFiles)'" />
    </ItemGroup>
    <Message
      Importance="High"
      Text="Building resource pack %(ResourcePackTask.Filename)%(ResourcePackTask.Extension)" />
    <ResourcePackTask
      Condition="'@(ResourcePackTask)' != '' and '%(ResourcePackTask.ExcludedFromBuild)' != 'true'"
      CommandLineTemplate="%(ResourcePackTask.CommandLineTemplate)"
      ComputeHashes="%(ResourcePackTask.ComputeHashes)"
      SuppressJson="%(ResourcePackTask.SuppressJson)"
      BuildLog="%(ResourcePackTask.BuildLog)"
      AdditionalOptions="%(ResourcePackTask.AdditionalOptions)"
      Inputs="%(ResourcePackTask.Identity)" />
  </Target>
 
</Project>
```

<!-- wp:paragraph -->
<p>This is the last piece of the file, defining one more Target. This is the target that actually does the heavy lifting, and you'll see the recurrence of the _ResourcePackTask name from earlier. There are two properties BeforeTargets and AfterTargets (not used here) that set when in the build process this target should run. It also takes a dependency on the tlog target above, so that's how that target is pulled in. Again there is some boilerplate here, but we start the actual build by simply outputting a message that reports what file we're compiling.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Lastly, the ResourcePackTask entry here constructs the execution of the task itself. I <em>think</em> that %(ResourcePackTask.Whatever) here has the effect of copying the definitions from the .props file into the task itself; the interaction between these three files doesn't seem especially well documented. In any case what seems to work is simply repeating all of your properties from the .props into the ResourcePackTask and they magically appear in the build. Here's a complete code listing for the file.</p>
<!-- /wp:paragraph -->

```xml
<?xml version="1.0" encoding="utf-8"?>
 
<!-- This file provides a VS build step for Kata resource pack files -->
<!-- See http://reedbeta.com/blog/custom-toolchain-with-msbuild/ for an overview of what's happening here -->
 
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
 
  <ItemGroup>
    <PropertyPageSchema
      Include="$(MSBuildThisFileDirectory)$(MSBuildThisFileName).xml" />
    <AvailableItemName Include="ResourcePackTask">
      <Targets>_ResourcePackTask</Targets>
    </AvailableItemName>
  </ItemGroup>
  <UsingTask
    TaskName="ResourcePackTask"
    TaskFactory="XamlTaskFactory"
    AssemblyName="Microsoft.Build.Tasks.v4.0, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a">
    <Task>$(MSBuildThisFileDirectory)$(MSBuildThisFileName).xml</Task>
  </UsingTask>
 
  <Target Name="_WriteResourcePackTaskTlogs"
        Condition="'@(ResourcePackTask)' != '' and '@(SelectedFiles)' == ''">
    <ItemGroup>
      <_ResourcePackTaskReadTlog Include="^%(ResourcePackTask.FullPath);%(ResourcePackTask.AdditionalDependencies)"
                     Condition="'%(ResourcePackTask.ExcludedFromBuild)' != 'true' and '%(ResourcePackTask.ManifestFileName)' != ''"/>
      <!-- This is the important line to configure correctly for tlogs -->
      <_ResourcePackTaskWriteTlog Include="^%(ResourcePackTask.FullPath);$([MSBuild]::NormalizePath('$(OutDir)Resources', '%(ResourcePackTask.ManifestFileName)'))"
                      Condition="'%(ResourcePackTask.ExcludedFromBuild)' != 'true' and '%(ResourcePackTask.ManifestFileName)' != ''"/>
    </ItemGroup>
 
    <WriteLinesToFile
      Condition="'@(_ResourcePackTaskReadTlog)' != ''"
      File="$(TLogLocation)ResourcePackTask.read.1u.tlog"
      Lines="@(_ResourcePackTaskReadTlog->MetaData('Identity')->ToUpperInvariant());"
      Overwrite="true"
      Encoding="Unicode"/>
    <WriteLinesToFile
      Condition="'@(_ResourcePackTaskWriteTlog)' != ''"
      File="$(TLogLocation)ResourcePackTask.write.1u.tlog"
      Lines="@(_ResourcePackTaskWriteTlog->MetaData('Identity')->ToUpperInvariant());"
      Overwrite="true"
      Encoding="Unicode"/>
 
    <ItemGroup>
      <_ResourcePackTaskReadTlog Remove="@(_ResourcePackTaskReadTlog)" />
      <_ResourcePackTaskWriteTlog Remove="@(_ResourcePackTaskWriteTlog)" />
    </ItemGroup>
  </Target>
 
  <Target
    Name="_ResourcePackTask"
    BeforeTargets="ClCompile"
    Condition="'@(ResourcePackTask)' != ''"
    Outputs="%(ResourcePackTask.ManifestFileName)"
    Inputs="%(ResourcePackTask.Identity)"
    DependsOnTargets="_WriteResourcePackTaskTlogs;_SelectedFiles"
    >
    <ItemGroup Condition="'@(SelectedFiles)' != ''">
      <ResourcePackTask Remove="@(ResourcePackTask)" Condition="'%(Identity)' != '@(SelectedFiles)'" />
    </ItemGroup>
    <Message
      Importance="High"
      Text="Building resource pack %(ResourcePackTask.Filename)%(ResourcePackTask.Extension)" />
    <ResourcePackTask
      Condition="'@(ResourcePackTask)' != '' and '%(ResourcePackTask.ExcludedFromBuild)' != 'true'"
      CommandLineTemplate="%(ResourcePackTask.CommandLineTemplate)"
      ComputeHashes="%(ResourcePackTask.ComputeHashes)"
      SuppressJson="%(ResourcePackTask.SuppressJson)"
      BuildLog="%(ResourcePackTask.BuildLog)"
      AdditionalOptions="%(ResourcePackTask.AdditionalOptions)"
      Inputs="%(ResourcePackTask.Identity)" />
  </Target>
 
</Project>
```

<!-- wp:paragraph -->
<p>With all of that in place, hypothetically Visual Studio will treat your fancy new file type and its attendant compile chain exactly how you want. There are probably still many improvements to be made - in particular, this scheme as written seems to suppress stdout from the console at the default "Minimal" MSBuild verbosity, which is something I haven't dug into. But this is a solid start for a fully integrated build process.</p>
<!-- /wp:paragraph -->
