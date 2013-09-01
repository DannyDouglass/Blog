---
author: DannyDouglass
date: '2010-08-02 19:12:14'
layout: post
slug: mercurial-create-remote-repositories-using-powershell
status: publish
title: 'Mercurial: Create Remote Repositories Using PowerShell'
---

In case you are not familiar with Mercurial (Hg), it is a Distributed Version Control System (DVCS) aimed at simplifying revision control.  The purpose of this post is not to introduce Mercurial, but you can read more about Mercurial at the following links to get better acquainted with the software:

  * [HgInit](http://hginit.com/) | [Joel Spolsky’s](http://www.joelonsoftware.com/) detailed introduction to Mercurial 
  * [Mercurial: The Definitive Guide](http://hgbook.red-bean.com/read/) | A comprehensive guide to using Mercurial 
  * [Mercurial vs. Git Comparison](http://code.google.com/p/support/wiki/DVCSAnalysis) | Google Code wiki analysis of two DVCS solutions 
  * [Official Mercurial Wiki](http://mercurial.selenic.com/wiki/) | Selenic’s official Mercurial wiki and guides 

Now that we are all comfortable with Hg (the chemical symbol for Mercury and nickname for Mercurial), I would like to take present an issue I spent some time working on today. <!-- more --> There are several different [publishing mechanisms](http://mercurial.selenic.com/wiki/PublishingRepositories) in Mercurial, but for the setup at my company we decided to simply use http being that our provider is only internally available and protected by several firewalls and other security measures.  We chose this approach over the more popular SSH publishing mechanism being that we are hosting our Mercurial installation on a Windows 2008 R2 server.  Had we installed it on a box running something other than Windows, SSH would have solved the problem I’m about to describe below.  Here’s a quick view of our basic setup:

[![Mercurial Setup Over Http](/images/2010-08-02-mercurial-create-remote-repositories-using-powershell/MercurialSetupOverHttp_thumb.png)](http://dannydouglass.com/images/2010-08-02-mercurial-create-remote-repositories-using-powershell/MercurialSetupOverHttp_thumb.png)

One limitation when using http as a publishing mechanism is that you [cannot create a repository remotely](http://ry4an.org/unblog/UnBlog/2009-09-17); no if, ands, or buts about it.  Another twist was that our CM (Configuration Management) procedures dictate that our developers are not permitted to RDP into the Mercurial server.  This left us trying to decide how to allow developers to create repositories remotely since our http communication channel does not allow remote repository creation.

[Troy Goode](http://SquaredRoot.com), a good friend and colleague, suggested that we leverage PowerShell to solve the problem.  After spending a little bit of time with the Configuration Manager a PowerShell solution was in place. Using the following basic PowerShell command, a remote session was initiated without making the developer an administrator on the machine with RDP access:

    ENTER-PSSession your-server-name-or-ipaddress
    
I originally encountered an **Access Denied** error, which we discovered was caused by my account not have the appropriate permissions on the Mercurial server to create a remote PowerShell session.  This is pretty easy to remedy and you can read more about it on the [TechNet library page](http://technet.microsoft.com/en-us/library/dd819508.aspx).  Simply execute the following cmdlet _on the server_ to manage the PowerShell remote session security configuration:
    
    set-pssessionConfiguration -name Microsoft.PowerShell -showSecurityDescriptorUI
    
The last change is to set the appropriate directory permissions on the remote server to the location where developers are permitted to create repositories. That should be straightforward enough without going into any examples since repository architectures can differ drastically.

It can’t be that easy, can it? Well, good news – it is that simple!  With those changes you should be able to remote manage (create and delete) your Mercurial (Hg) repositories via PowerShell.  Hopefully this helps you save some time in your setup of Mercurial – a very awesome, and my current favorite, Version Control System.