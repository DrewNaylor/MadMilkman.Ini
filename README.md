> Note: This fork ports MadMilkman.Ini to .NET Standard 2.0. Documentation may take a bit to update to reflect this.

> Note 2: Neither this project nor myself are associated with Microsoft or .NET, and Microsoft does not condone this project. Adding "dotnet" to the project's name is only to show that it works with .NET.

> Note 3: Although this project is licensed under the MIT License as stated in the "LICENSE" file, some files are licensed under the Apache License, Version 2.0. To be safe, I recommend ensuring your project's license is compatible with both the MIT License and the Apache License, Version 2.0. More details: https://github.com/DrewNaylor/libinidotnet/issues/7

# libinidotnet, a fork of MadMilkman.Ini built for .NET Standard 2.0
This **[fork of MadMilkman.Ini](https://github.com/MarioZ/MadMilkman.Ini)** is a .NET Standard 2.0 library which simplifies processing of INI files and requires only a minimum of any of these [.NET versions](https://docs.microsoft.com/en-us/dotnet/standard/net-standard):
- .NET Framework 4.6.1 (4.7.2 is recommended as 4.6.1 has issues with using .NET Standard 1.5-2.0 libraries)
- .NET 5.0
- .NET Core 2.0
- Mono 5.4

There may be other .NET implementations supported, but they may not work.<br>
It is 100% managed code (C#), which provides an easy to use programming interface.

## Advantages
> This part was unchanged.<br>
* Enables reading and writing of various INI file formats.
* Enables easy manipulation of INI file's content.
* Enables copying and merging multiple INI file's contents.
* Enables encrypting and decrypting INI files.
* Enables compressing and decompressing INI files.
* Enables serializing and deserializing custom types into an INI content.

## Installation
You can use this library by referencing MadMilkman.Ini.dll inside your project after extracting it from the [ZIP file](https://github.com/DrewNaylor/MadMilkman.Ini/releases/latest).

## First steps:
1. Create new .NET project.
2. Add new reference to MadMilkman.Ini.dll.
3. Add MadMilkman.Ini namespace.
  * C# - `using MadMilkman.Ini;`
  * VB.NET - `Imports MadMilkman.Ini`
  * C++/CLI - `using namespace MadMilkman::Ini;`
  > I'm not entirely sure if this'll work for C++/CLI as VS2019 said it couldn't find the correct program for it, but I'm keeping this here as it was in the original readme and could work.
4. Write some INI files processing code.
  * Use code samples located in MadMilkman.Ini.Samples, written in C#, VB.NET and C++/CLI as starting point.
  > Again, I didn't update the C++ project, so it may not work.
  * Read [MadMilkman.Ini.Documentation.chm](https://github.com/DrewNaylor/MadMilkman.Ini/raw/master/MadMilkman.Ini.Documentation.zip) to learn more about the component and its API references.

## Feedback & Support
> From the original readme, so I might not be able to help with questions or other stuff, and [MarioZ](https://github.com/MarioZ/MadMilkman.Ini) is the original author of the library, so feedback that's unrelated to this fork should go to them.<br>

Please feel free to contact me with any questions, suggestions or issues regarding the MadMilkman.Ini component, I will be more than happy to provide a help.
Also if you found the component useful or useless I would be interested in hearing about it.

## Overview
> This part was unchanged.<br>

MadMilkman.Ini provides a simple and intuitive programming interface which makes it very easy to create new or process existing INI files. Because INI file format is loosely defined and has no real standard, different files can have different format. By default MadMilkman.Ini processes the following format (however it is possible to define a custom formatting via IniOptions class):

```cfg
;Section trailing comment
[Section name]
Key name = Key value ;Key leading comment
```

### C# #
```csharp
// Create new file with a default formatting.
IniFile file = new IniFile();

// Add new section.
IniSection section = file.Sections.Add("Section name");
// Add trailing comment.
section.TrailingComment.Text = "Section trailing comment";

// Add new key and its value.
IniKey key = section.Keys.Add("Key name", "Key value ");
// Add leading comment.
key.LeadingComment.Text = "Key leading comment";
            
// Save file.
file.Save("Sample.ini");
```

### VB.NET
```vb.net
' Create new file with a default formatting.
Dim file As New IniFile()

' Add new section.
Dim section As IniSection = file.Sections.Add("Section name")
' Add trailing comment.
section.TrailingComment.Text = "Section trailing comment"

' Add new key and its value.
Dim key As IniKey = section.Keys.Add("Key name", "Key value ")
' Add leading comment.
key.LeadingComment.Text = "Key leading comment"

' Save file.
file.Save("Sample.ini")
```

### C++/CLI
```cpp
// Create new file with a default formatting.
IniFile^ file = gcnew IniFile();

// Add new section.
IniSection^ section = file->Sections->Add("Section name");
// Add trailing comment.
section->TrailingComment->Text = "Section trailing comment";

// Add new key and its value.
IniKey^ key = section->Keys->Add("Key name", "Key value ");
// Add leading comment.
key->LeadingComment->Text = "Key leading comment";

// Save file.
file->Save("Sample.ini");
```

**Compression** feature enables you to reduce INI file's size.

**Encryption** feature enables you to protect INI file by providing an encryption password.

**Parsing** feature enables you to retrieve the key's value as an instance of a specific type via key's TryParseValue method, all currently supported types are listed in key's IsSupportedValueType method remarks.

**Binding** feature enables you to define placeholders inside a key's value which can be binded (replaced) with an internal or external data source via Bind() method.

**Serialization** feature enables you to serialize an object into section's keys.

## List Sections
> The following is mostly copied (with a few small changes) from [JohnDovey's pull request adding to the docs](https://github.com/MarioZ/MadMilkman.Ini/pull/32), as I figured it would be useful to include it in some form so it's not lost. Please be aware that this code is **untested** and **may not work without modification**, but it should be enough to get started. I may add a C# example in the future.

If you wish to iterate through the file and retrieve a list of sections, you can do something like this:

### VB.NET

```vbnet
Dim iniFile As New Ini.IniFile(                
New Ini.IniOptions() With {                    
    .CommentStarter = Ini.IniCommentStarter.Semicolon,                    
    .KeyDelimiter = Ini.IniKeyDelimiter.Colon,                    
    .KeySpaceAroundDelimiter = False,                    
    .SectionWrapper = Ini.IniSectionWrapper.SquareBrackets})
        
    ' Load file from path.        
    iniFile.Load("C:\testfile.ini")        
    
    Dim SectionsCount As Integer = iniFile.Sections.Count
        
   ' Iterate through the INI file returning all the section names.        
   For x As Integer = 0 To SectionsCount - 1            
       Console.WriteLine(iniFile.Sections(x).Name)        
   Next
```

## More Documentation
More details can be found in [MadMilkman.Ini.Documentation.chm](https://github.com/DrewNaylor/libinidotnet/blob/master/docs/MadMilkman.Ini.Documentation.zip?raw=true). The info in this CHM file will be moved to a GitHub Wiki at some point, as Windows HTML Help is deprecated I think, and I'd like to preserve the documentation from the original project. If that's too difficult, I'll just put it into files in /docs.
