---
author: DannyDouglass
date: '2010-10-27 07:52:08'
layout: post
slug: console2powershellposhhg
status: publish
title: 'A Better Hg Command Line Interface: Console2 + PowerShell + Posh-Hg'
---

Moving from Subversion to [Mercurial](http://mercurial.selenic.com/) was more than a change from a centralized version control system to a distributed solution.  I also found myself moving away from the explorer integration provided by the [Tortoise](http://tortoisehg.bitbucket.org/) products to the command line for the execution of my hg commands.  This approach is dramatically faster once you get used to the syntax, which really only takes a hot minute.

Certain features in [TortoiseHg](http://tortoisehg.bitbucket.org/) still provide visual advantages that I find useful, such as the visual repository log.  During an Agile.Net Bootcamp with [Jimmy Bogard](http://www.lostechies.com/blogs/jimmy_bogard/) he showed us an collection of tools he utilizes to provide an improved Mercurial command line experience.  The final product is a more descriptive prompt that displays branch information, as well as the number and types of changes made since your last commit:

[![posh-hg](/images/2010-10-27-console2powershellposhhg/poshhg_thumb.png)](http://dannydouglass.com/images/2010-10-27-console2powershellposhhg/poshhg_thumb.png)

### 1. Console 2

[http://sourceforge.net/projects/console/](http://sourceforge.net/projects/console/)

You may already being using this very useful command prompt shell, but in case you are not go download it now!  This tool provides certain advantages over a standard command prompt, such as tabbed prompts, ability to integrate other shells (e.g. PowerShell), modified styles, control over keyboard commands, and much more.  Once you have downloaded console 2, just extract it somewhere (I used my c:\Program Files directory).

### 2. Windows PowerShell

You can read more on Windows PowerShell [here](http://technet.microsoft.com/en-us/scriptcenter/powershell.aspx) and [here](http://technet.microsoft.com/en-us/library/ee692944.aspx).  It comes with Windows 7, but you also [download](http://www.microsoft.com/windowsserver2003/technologies/management/powershell/download.mspx) it for other versions of Windows.  If you do not have it, download and install Windows PowerShell.

After installing PowerShell, we want to create a physical PowerShell profile file.  The default location on Windows 7 is _c:/users/username/document/WindowsPowerShell/_ and you can force the creation of your PowerShell profile file by executing the following command in a PowerShell prompt:
    
    New-Item -path $profile -type file -force

After executing that command you can verify the file was created:  

[![PowerShellProfileCreation](/images/2010-10-27-console2powershellposhhg/PowerShellProfileCreation_thumb.png)](http://dannydouglass.com/images/2010-10-27-console2powershellposhhg/PowerShellProfileCreation_thumb.png)

### 3. Posh-Hg Download and PowerShell Integration

[http://poshhg.codeplex.com/](http://poshhg.codeplex.com/)
  
Posh-Hg was inspired by the [Posh-Git](http://github.com/dahlbyk/posh-git), which provides a “custom prompt and tab expansion when using Mercurial from within a Windows PowerShell command line”.  There’s more information on this [blog post](http://www.jeremyskinner.co.uk/2010/04/21/using-mercurial-with-windows-powershell/) about Posh-Hg.  You will want to download this file and place it somewhere on disk.  **Important**:  Before unzipping, make sure to right-click on the posh-hg-1.0 zip file and **Unblock** it, otherwise you will run into security issues executing the scripts each and every time (very, very annoying):  

[![Capture](/images/2010-10-27-console2powershellposhhg/Capture_thumb.png)](http://dannydouglass.com/images/2010-10-27-console2powershellposhhg/Capture_thumb.png)

Once you’ve unblocked the file you need to unzip it.  I recommend saving it the same location as your Windows PowerShell profile.  In my case that was _C:\Users\Danny\Documents\WindowsPowerShell_.  Now you are set to add posh-hg to you PowerShell profile.  Simply open your PowerShell profile file and add the following text to it, making sure to modify the path to the posh-hg _profile.example.ps1_ file appropriately:

    . path\to\posh-hg\profile.example.ps1

You can now open a new PowerShell prompt and verify the installation by navigating to a Hg repository on disk.  You may receive an error explaining that your profile “_cannot be loaded because the execution of scripts is disabled on this system_”.  If that happens you will need to adjust the PowerShell script policy by executing the following command (type [Y] when prompted):

    Set-ExecutionPolicy Unrestricted

If you needed to set your execution policy, you will need to close your current PowerShell prompt and open a new one for the changes to take affect and allow for your profile to be loaded.  Try navigation to a Hg repository on disk and you’ll see a much more friendly prompt:

[![Capture2](/images/2010-10-27-console2powershellposhhg/Capture2_thumb.png)](http://dannydouglass.com/images/2010-10-27-console2powershellposhhg/Capture2_thumb.png)

### 4. PowerShell Integration with Console2 and Posh-Hg

With everything installed and configured outside of Console2 we are set to add PowerShell to our Console2 installation.  You can [download](http://DannyDouglass.com/wp-content/uploads/2010/10/console2-powershell-poshhg.zip) the files to update your Console2 installation (the console.xml file).  If you have not customized anything previously with Console2, you can just override the existing file without causing any harm. Drop the powershell.ico icon file in the Console2 directory and you are ready to test out the changes in Console2.  Open your console, navigate to a Mercurial repository on disk and you should see the changes:

[![Capture](/images/2010-10-27-console2powershellposhhg/Capture_thumb1.png)](http://dannydouglass.com/images/2010-10-27-console2powershellposhhg/Capture_thumb1.png)

Here is a breakdown of the meaning the symbol/number combinations:
  
  * “+<number>” – # of files that have been added (not committed) 
  * “~<number>” – # of files that have been modified (not committed) 
  * “-<number>” – # of files that have been removed (not committed) 
  * “?<number>” – # of untracked files in your repo 
  * “!<number>” – # of missing files 
  
You can edit how the Hg details are display as well.  Take a look at the [posh-hg codeplex page](http://poshhg.codeplex.com/) for those details.

This [blog post](http://www.vcritical.com/2009/02/better-console-for-powershell-and-vitk/) helped me accomplish the 4th step of my install and provided the zip file (which I slightly modified) used to update the console2 configuration.

### Done!

You are now setup with a significantly more useful PowerShell command prompt that can help you visually get a feel for the state of your repository immediately.  Of course, I’m positive you [check in early and often](http://www.codinghorror.com/blog/2008/08/check-in-early-check-in-often.html), which results in this information being substantially more helpful.  If I’m always showing changes to 50 - 100+ files, how much does that really help me???

**Added Bonus: **Take a look at ScottHa’s [blog post](http://www.hanselman.com/blog/IntroducingPowerShellPromptHere.aspx) on adding a “PowerShell Prompt Here” context menu item for Windows Explorer.  Very Handy!

Hope you enjoy this as much as I did – thanks to [Jimmy Bogard](www.lostechies.com/blogs/jimmy_bogard/) for the exposure to Posh-Hg!  