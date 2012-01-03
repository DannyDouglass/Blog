---
author: DannyDouglass
date: '2010-08-09 09:20:36'
layout: post
slug: whats-new-in-the-dotnet-world
status: publish
title: What's Nu in the .NET World?
wordpress_id: '183'
? ''
: - Articles
  - Nu
  - Open Source
---

If you thought the word "_Nu_" in the title of this blog post was misspelled, you are in for a treat.  And trust me, you are not the only one who made that grammatical assumption.

**Nu **is an open source tool built by [Dru Sellers](http://codebetter.com/blogs/dru.sellers/default.aspx) that aims at bringing [Gems](http://rubygems.org/), one of Ruby’s most revered features, to the .NET world.  If you are not familiar with Gems I suggest you take a minute to [read up on them](http://docs.rubygems.org/read/book/3).  I would venture a bet that you are already familiar with at least one gem – [Ruby on Rails](http://rubyonrails.org).  The following line of code is all that is required to install the Ruby on Rails gem (after installing the Ruby library of course):
    
    gem install rails 

Compare that to what is required to install ASP.NET MVC.  Despite having ASP.NET installed, I may need to download a separate installer for a specific version of Visual Studio.  Semantics aside, it is just plain more difficult. That is not to say installing ASP.NET MVC is overly complex, nor am I attempting to disrespect the ASP.NET MVC framework.  I’m actually a _big fan_ of ASP.NET MVC. My intention is to show that the gem installation is just _crazy simple** **_and a piece of the programmatic puzzle missing in .NET development.  Many, including myself, are hoping that **Nu** proves to be the solution to this shortcoming.

This is not the first time someone has attempted to solve the “Missing Gem” (as I have decided to label it) mystery in the .NET world.  I recall my friend [Troy Goode](http://SquaredRoot.com) discussing this exact problem with [Stephen Walther](http://stephenwalther.com/blog/default.aspx) just prior to him joining Microsoft way back in early 2008.  Many others have tried to **replace** Ruby Gems in the .NET world, but none have been successful.  [Dru Sellers](http://codebetter.com/blogs/dru.sellers/default.aspx) decided to take a different approach and leverage the existing Ruby Gems tools and expand upon it to bring gems to .NET.  Brilliant.

Enough discussion about Nu – I will let the code speak for itself.  In the following example I will create a basic ASP.NET MVC 2 web application (.NET 4.0) and use the Nu package installer to hook in [AutoFac](http://code.google.com/p/autofac/), a .NET [IoC container](http://martinfowler.com/articles/injection.html).  Do not be alarmed if you do not know what AutoFac or IoC are at this point.  Just understand that I have an external component that I would like to utilize in my project.  For a list of all the currently available Nu packages you can take a look at the [Current .NET Nu Packages](http://nu.wikispot.org/Current_Packages) page on the Nu wiki.  Now let’s see the code!

**Note**: You must [install Ruby](http://rubyinstaller.org/downloads/) as a prerequisite to using Nu since it leverages Ruby’s Gem tool.

The first step is to create a new ASP.NET MVC 2 application:

[![image](/images/2010-08-09-whats-new-in-the-dotnet-world/image_thumb.png)](http://dannydouglass.com/images/2010-08-09-whats-new-in-the-dotnet-world/image_thumb.png))

Once the application is created I can quickly run it and see the screen that anyone who has read about or used ASP.NET MVC before is very familiar with:

[![image](/images/2010-08-09-whats-new-in-the-dotnet-world/image_thumb1.png)](http://dannydouglass.com/images/2010-08-09-whats-new-in-the-dotnet-world/image_thumb1.png)

Now that I have my project created I want to download the AutoFac library and bring it into my project.  Prior to Nu, I would do this by opening a browser, finding the AutoFac download page, download, possibly unzip the files, etc. Nu makes this much simpler.  First thing I want to do install Nu using the Ruby gem tool by simply executing the command **gem install nu**.

[![NuGemInstall](/images/2010-08-09-whats-new-in-the-dotnet-world/NuGemInstall_thumb.png)](http://dannydouglass.com/images/2010-08-09-whats-new-in-the-dotnet-world/NuGemInstall_thumb.png)

Once I have installed Nu, I can easily download the AutoFac gem by executing the command **nu install autofac**.
 
[![image](/images/2010-08-09-whats-new-in-the-dotnet-world/image_thumb2.png)](http://dannydouglass.com/images/2010-08-09-whats-new-in-the-dotnet-world/image_thumb2.png)

Note that by default Nu will install your gems local (in a new _lib_ directory) to the folder where you execute the **nu install #somegem#** command.  Taking a look at my _c:\projects\Nu_AspNetMvc2_Demo_ directory I can see that two folders were created: ._nu_ and _lib_.  The _lib _folder will hold all of the gems I install in this directory from here on out, unless I override the default at a later time.  I will not go into further details, but my AutoFac dlls are now stored inside of my _/lib/autofac _package directory.You can take a look at the [example’s source](http://dl.dropbox.com/u/5632950/Nu_AspNetMvc2_Demo.zip) to see more about the package directory structure.  

[![NuAutofacInstallFolders](/images/2010-08-09-whats-new-in-the-dotnet-world/NuAutofacInstallFolders_thumb.png)](http://dannydouglass.com/images/2010-08-09-whats-new-in-the-dotnet-world/NuAutofacInstallFolders_thumb.png)

Currently Nu is in its early stages and I am still required to add the DLLs to my project manually, but very soon this will not be the case.  A Nu for Visual Studio plugin is in the works that will simplify this step – take a look at [this video preview](http://www.youtube.com/watch?v=0-1UPrKu3wg) to see it in action.  I imagine there will be other ways to accomplish this as well.  For now, I will simply add the required references as I would with any other library I downloaded to my Visual Studio Project:

[![ProjectReferences](/images/2010-08-09-whats-new-in-the-dotnet-world/ProjectReferences_thumb.png)](http://dannydouglass.com/images/2010-08-09-whats-new-in-the-dotnet-world/ProjectReferences_thumb.png)

Now all I need to do is follow [these instructions](http://code.google.com/p/autofac/wiki/MvcIntegration) to hookup AutoFac 2 in my _global.asax.cs (code shown below)_ and I am good to go! Running my project shows me that same familiar MVC homepage, however this time my controller factory is running using the AutoFac controller factory (see line 33 below):
    
    using System.Reflection;
    using System.Web;
    using System.Web.Mvc;
    using System.Web.Routing;
    using Autofac;
    using Autofac.Integration.Web;
    using Autofac.Integration.Web.Mvc;  
    namespace Nu_AspNetMvc2_Demo
    {
        public class MvcApplication : HttpApplication, IContainerProviderAccessor
        {
            private static IContainerProvider _containerProvider;  
            public static void RegisterRoutes(RouteCollection routes)
            {
                routes.IgnoreRoute("{resource}.axd/{*pathInfo}");  
                routes.MapRoute(
                    "Default", // Route name
                    "{controller}/{action}/{id}", // URL with parameters
                    new { controller = "Home", action = "Index", id = UrlParameter.Optional } // Parameter defaults
                );  
            }  
            protected void Application_Start()
            {
                var builder = new ContainerBuilder();
                builder.RegisterControllers(Assembly.GetExecutingAssembly());
                
                _containerProvider = new ContainerProvider(builder.Build());
                ControllerBuilder.Current.SetControllerFactory(new AutofacControllerFactory(ContainerProvider));  
                AreaRegistration.RegisterAllAreas();
                RegisterRoutes(RouteTable.Routes);
            }  
            public IContainerProvider ContainerProvider
            {
                get { return _containerProvider; }
            }
        }
    }

As you can see there is a lot of power in using Nu.  There are many other areas I could have gone into further detail, but I wanted this to be a simple introduction to the power of Nu packages.  I plan on following up with additional posts to show other features of Nu and maybe even a post on creating your very own Gem.  In the meantime I want to include a few resources to help you learn more about Nu:

  1. **Nu Wiki | **[http://nu.wikispot.org](http://nu.wikispot.org/)
  
  2. **Nu Google Group |** [http://groups.google.com/group/nu-net](http://groups.google.com/group/nu-net)
  
  3. **The Power of Nu Video** **|** [http://www.youtube.com/watch?v=IvxAa4XURss&feature=youtu.be](http://www.youtube.com/watch?v=IvxAa4XURss&feature=youtu.be)
  
**[Download this Example’s Source Code](http://dannydouglass.com/downloads/Nu_AspNetMvc2_Demo.zip)**

[![Shout it](http://dotnetshoutout.com/image.axd?url=http%3A%2F%2Fdannydouglass.com%2F2010%2F08%2Fwhats-new-in-the-dotnet-world%2F)](http://dotnetshoutout.com/Whats-Nu-in-the-NET-World-DannyDouglasscom)