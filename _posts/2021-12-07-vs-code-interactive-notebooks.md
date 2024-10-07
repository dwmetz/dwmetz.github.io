---
layout: post
permalink: http://bakerstreetforensics.com/2021/12/07/vs-code-interactive-notebooks/
title: VS Code Interactive Notebooks
description: None
date: 2021-12-07 22:24:09 -0000
last_modified_at: 2021-12-07 22:24:09 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
tags:
- Memory
- Mind Palace
- Python
- VSCode
---
I've been using Visual Studio Code as my go to editor for PowerShell, JSON, plain text, and recently even a dash of Python. VS Code is very extensible and much like the App Stores we've come to know, there's an extension marketplace to broaden its capabilites.

One of my favorite extensions is the [.NET Interactive Notebooks](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.dotnet-interactive-vscode). Notebooks combine markdown text and code snippets that you can run right within the notebook. This can be very useful for designing playbooks for a SOC or Junior Analyst to execute as you can describe and provide guidance on how to utilize the code functions.

An easy way to get started with Interactive Notebooks is to create a "Quick Codes" notebook. Title it as you choose. For this particular notebook, I've got a number of commands saved that I may reference semi-frequently, but due to limited space in my [mind palace](https://www.smithsonianmag.com/arts-culture/secrets-sherlocks-mind-palace-180949567/) I wind up googling them anyway, even if it's [googling my own site](https://duckduckgo.com/?q=site%3A+bakerstreetforensics.com+WSL&ia=web). 

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/mind-palace.jpg?w=470)Trying to remember a specific PowerShell syntax

**Note before installing** :  
_As your scripts and notebooks develop, there is a likelihood that you will want to run some either as Administrator or using another user credential. One way to do so simply launch VS code (right click) as Admin, or use the Run As feature when you launch the application._

  1. Download and install VS Code.  
Note - as you may be running this with multiple credentials, the "System" installer is recommended.  
<https://code.visualstudio.com/Download#>
  2. Install the latest .Net SDK  
<https://dotnet.microsoft.com/download/dotnet/6.0>
  3. When inside VS code, bring up the Extensions view by clicking on the Extensions icon in the Activity Bar on the side of VS Code, or the View: Extensions command (Ctrl+Shift+X).  
Search for "interactive"  
Select **.NET Interactive Notebook** s and choose install



Once everything is all set, relaunch VS Code. 

Hit **Ctrl+Shift+P** and select**.NET Interactive - Create New Blank Notebook.**

That's it. Now start adding blocks for text and code. You can use simple markup codes for Heading (#), Heading 2 (##), Heading 3 (###), etc.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/vscodenb.png?w=1024)

To execute the code snippet, just click on the small 'play' arrow to the left.

Do you have any novel uses for Interactive Notebooks? If so, please share in the comments area.
