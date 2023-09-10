---
layout: post
title: Dotnet core template! 🧾
category: posts
lang: en
tags: [dotnet, core, dev, study, template]
image: /assets/img/posts/dotnet.webp
accent_image: 
  background: url('/assets/img/posts/dotnet-small.jpg') center 
  overlay: true
description: >
  How to create a dotnet core template to be used via the command line or visual studio.
invert_sidebar: false
---

Standardizing the code across a team or even several teams can be a challenge. One of the facilities that dotnet offers is the creation of templates, with which it is possible to generate a suggested structure, making it easier to start a new project.

In the past, it was necessary to use a specific nomenclature for the namespace or variables that needed to be replaced when generating the template, this made the entire process of testing, building and then generating the template difficult. In new versions this is no longer necessary, making it much simpler the generation of templates.

To start we will need some commands, basically we will create a folder for the project and the solution file ```.sln```.

```bash
mkdir DotnetTemplateTest
cd DotnetTemplateTest
dotnet new sln -n DotnetTemplateTest
```

Then we will create the folder ```Content```, in it we will create the initial project and the template settings, in this case I will use a ```Console Application``` but you can change it to how you want to start your template project.

```bash
mkdir Content
cd Content
dotnet new console -o DotnetTemplateTest
cd ..
dotnet sln add Content/DotnetTemplateTest
```

So far we have a folder structure like this:

```
└───DotnetTemplateTest
    │   DotnetTemplateTest.sln
    │   Content/
    │
    └───DotnetTemplateTest
          DotnetTemplateTest.csproj
          Program.cs
```

Now we can run the project to test if everything is working so far with the command:

```
dotnet run --project Content/DotnetTemplateTest
```

And let's make a simple change to demonstrate how replace works when generating a new template, in this case I will replace the ```Hello, World!``` for ```Hello, ```**```DotnetTemplateTest```**```!```. Note that I gave the same name as the project and solution on purpose to be replaced!

Everything is ready, our project is working, now it's time to generate the template, for this we will create the file ```template.json``` on the following path ```Content/.template.config``` with the following content:

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Luigi Henrick",                              // Your name here
  "classifications": [ "Common", "Console" ],             // Tags to find the template
  "identity": "LuigiHenrick.DotnetTemplateTest.CSharp",   // Identifier for the template
  "name": "Dotnet Template Test",                         // Name as it will be displayed in the list
  "shortName": "templatetest",                            // Alias ​​to use via command line
  "sourceName": "DotnetTemplateTest"                      // Project name (The replace will be by this name)
}
```

With the file saved, we can now install and test the template with the following commands:

```
dotnet new --install Content/DotnetTemplateTest/
dotnet new templatetest -o NewProject
dotnet run --project NewProject/NewProject.csproj
```

With everything right, we can now remove the project created to validate the template ```rm -r NewProject``` and done, now the template is ready to be used.

For more details about the template, I recommend the sayedihashimi repository:
 [template-sample](https://github.com/sayedihashimi/template-sample), and its presentation on the official Microsoft channel: [Youtube ▶️ ](https://www.youtube.com/watch?v=GDNcxU0_OuE)