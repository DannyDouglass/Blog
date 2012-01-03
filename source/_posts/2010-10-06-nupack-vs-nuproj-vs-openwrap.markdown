---
author: DannyDouglass
date: '2010-10-06 09:32:25'
layout: post
slug: nupack-vs-nuproj-vs-openwrap
status: publish
title: NuPack vs. Nu (Nubular) vs. OpenWrap
wordpress_id: '270'
? ''
: - ALT.NET
  - Articles
  - Nu
  - NuPack
  - OpenWrap
  - Package Management
---

The [ASP.NET MVC 3 Beta](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=0abac7a3-b302-4644-bd43-febf300b2c51) was unofficially released yesterday (as far as I can tell) and the release notes include a quote about a package manager named NuPack:

> ASP.NET MVC 3 includes NuPack Package Manager, which is an integrated package management tool for adding libraries and tools to Visual Studio projects. For the most part, it automates the steps that developers take todayto get a library into their source tree.

> You can work with NuPack as a command line tool, as an integrated console
window inside Visual Studio 2010, from the Visual Studio context menu, and as
set of PowerShell cmdlets.

> For more information about NuPack, visit [http://nupack.codeplex.com/](http://nupack.codeplex.com/) and read the [Getting Started Guide](http://nupack.codeplex.com/documentation?title=Getting%20Started).

Unfortunately, the CodePlex site is currently not published and many are waiting to take a look at what’s being released here by Microsoft.  There seems to be a good deal of confusion on Twitter revolving around this project so I thought a quick post outlining each of the 3 main package managers in .NET would be helpful.  I’ve listed them in alphabetical order:

**_Update_**:  Thanks to a comment below from Keith Dahlby…it appears that NuPack may be a joint effort between the creators of Nu and Microsoft.  You can read more about it on the [Outercurve Foundation announcement.](http://www.outercurve.org/News/articleType/ArticleView/articleId/20/Outercurve-Foundation-Announces-NuPack-as-Fifth-Project-in-ASPNET-Open-Source-Gallery)

**Update #2**: Microsoft has [announced](http://weblogs.asp.net/scottgu/archive/2010/10/06/announcing-nupack-asp-net-mvc-3-beta-and-webmatrix-beta-2.aspx) NuPack was named with the blessing of the Nu (Nubular) team and the projects appears to merging to some extent.  Nu will continue moving forward, but may ultimately die-off in favor of NuPack’s native .NET solution.

* _**Nu (Nubular)**:_ Nu is a project started by [Dru Sellers](http://codebetter.com/blogs/dru.sellers/default.aspx) and [Rob Reynolds](http://devlicio.us/blogs/rob_reynolds/default.aspx) that I have blogged about [here](http://dannydouglass.com/2010/08/whats-new-in-the-dotnet-world/) and [here](http://dannydouglass.com/2010/08/whats-nu-in-the-dotnet-world-part2-creating-a-gem/).  It leverages the Ruby gems platform to bring gems to the .NET world. 

Twitter hashtag **#NuProj**

  * **_NuPack_**: Microsoft’s package management tool included in the [ASP.NET MVC 3 Beta](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=0abac7a3-b302-4644-bd43-febf300b2c51).  The source is located on [CodePlex](http://nupack.codeplex.com/), but is not published at this time (9:15 EST). _Update_ – source is available!  See update #2 above. 

Twitter hashtag **#NuPack**

  * **_OpenWrap_**: A native .NET package management solution by [Sebastien Lambla](http://serialseb.blogspot.com/) that aims at managing dependencies in a manner that will allow runtime resolution to dependency issues as well. 

Twitter hashtag **#OpenWrap**

Without hearing more on NuPack’s approach to managing .NET packages it is hard to compare the 3.  I am sure that an announcement will come in the near future from someone on the ASP.NET team at Microsoft regarding the ASP.NET MVC3 beat and NuPack.  In the meantime, I hope this helps clarify the difference between the different .NET Package Managers.

Stay tuned for more…

