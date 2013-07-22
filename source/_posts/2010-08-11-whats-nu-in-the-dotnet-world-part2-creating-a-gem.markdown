---
author: DannyDouglass
date: '2010-08-11 20:39:37'
layout: post
slug: whats-nu-in-the-dotnet-world-part2-creating-a-gem
status: publish
title: 'What’s Nu in the .NET World Part 2: Creating a Gem'
---

If you are not familiar with the Nu project start by reading my [first post in this series](http://dannydouglass.com/2010/08/whats-new-in-the-dotnet-world/) that gives an introduction to getting started with this promising tool.  As a quick reference I’ll post a paragraph from the last post introducing Nu:

**“Nu **is an open source tool built by [Dru Sellers](http://codebetter.com/blogs/dru.sellers/default.aspx) (and several others) that aims at bringing [Gems](http://rubygems.org/), one of Ruby’s most revered features, to the .NET world.  If you are not familiar with Gems I suggest you take a minute to [read up on them](http://docs.rubygems.org/read/book/3).  I would venture a bet that you are already familiar with at least one gem – [Ruby on Rails](http://rubyonrails.org).  The following line of code is all that is required to install the Ruby on Rails gem (after installing the Ruby library of course)…”

This post is focused on sharing my experience in creating my first gem for the [Spark View Engine](http://sparkviewengine.com/).  I would be doing you a great disservice if I did not start by saying how easy it is to create a gem. Actually easy is the wrong phrase – **crazy easy** is more telling.  Since I would never ask you to take my word for it, it must mean it is time to show some code.

**_Getting Started_**

After downloading the latest release for the Spark View Engine [here](http://sparkviewengine.codeplex.com/releases/view/27601), the first step was to create a directory to hold my gem specification anywhere on my machine, c:/Gems/SparkViewEngine was my choice:

[![image](/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb3.png)](http://dannydouglass.com/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb3.png)

**_Creating the Gem Specification_ **(GemSpec)

[Rob Reynolds](http://devlicio.us/blogs/rob_reynolds/default.aspx) described this file as “the most important file for this entire process”.  The gemspec file will be named after the project – in this case it is named **sparkviewengine.gemspec** and will be placed inside my sparkviewengine directory.  While we are creating files we can also create a **VERSION** file that, you may have guessed, stores the version of the gem.  We now have a directory with 2 files:

[![image](/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb4.png)](http://dannydouglass.com/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb4.png)

Below is the code from my **sparkviewengine.gemspec** file, you would simply replace the values with those applicable to the gem you are creating:

    
    version = File.read(File.expand_path("../VERSION",__FILE__)).strip  
      
    Gem::Specification.new do |spec|  
      spec.platform    = Gem::Platform::RUBY  
      spec.name        = 'sparkviewengine'  
      spec.version     = version  
      spec.files 	   = Dir['lib/**/*'] + Dir['docs/**/*']  
      
      spec.summary     = 'Spark View Engine for Asp.Net Mvc and Castle Project MonoRail'  
      spec.description = <<-EOF
    	Spark is a view engine for Asp.Net Mvc and Castle Project MonoRail frameworks. 
    	The idea is to allow the html to dominate the flow and the code to fit seamlessly.
      EOF
        
      spec.authors           = 'Louis Dejardin'
      spec.email             = 'spark-dev@googlegroups.com'  
      spec.homepage          = 'http://sparkviewengine.com'  
      spec.rubyforge_project = 'sparkviewengine'  
    end  

  

The VERSION file is much simpler.  I found the version below by opening [Reflector](http://www.red-gate.com/products/reflector/) and locating the version of the Spark.dll.  Be sure to check out Rob Reynold’s [blog post](http://devlicio.us/blogs/rob_reynolds/archive/2010/07/17/how-to-gems- and-net-dependencies-references.aspx) for the explanation on why you need to use Reflector and cannot trust the windows explorer properties panel.
    
    1.1.0.0

**_Adding the Library Files_**  

The hard part is finished in creating our basic gem, now we need to include the spark compiled dlls.  For this example we are creating a basic gem and do not have to deal with dependencies.  My next gem will be more detailed and I will discuss what steps are necessary to [handle dependencies](http://devlicio.us/blogs/rob_reynolds/archive/2010/07/17/how-to-gems-and-net-dependencies-references.aspx).

For the SparkViewEngine gem we can simply copy the libraries from the bin directory of the Spark 1.1.0.0 release:

[![image](/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb5.png)](http://dannydouglass.com/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb5.png)
 
and place them into a new **lib** directory in my SparkViewEngine gem directory:

[![image](/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb6.png)](http://dannydouglass.com/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb6.png)

**Documentation**

Any documentation can be stored in a **docs** directory.  I added the
index.html and zipped up the samples that came with the Spark 1.1.0.0 release:

[![image](/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb7.png)](http://dannydouglass.com/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb7.png)

That’s it for the setup!  My gem specification is ready to be built and uploaded to [RubyGems.org](http://RubyGems.org).  

**_Building the Gem Specification_**
    
    gem build sparkviewengine.gemspec

[![image](/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb8.png)](http://dannydouglass.com/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb8.png)
  
That creates my versioned gem:

[![image](/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb9.png)](http://dannydouglass.com//images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb9.png)

**_Pushing the Gem to RubyGems.org_**

You need to have an account created on RubyGems.org before you can push.  Once that is created simply execute the following code, enter your credentials, and watch as your gem is pushed out to the world!
    
    gem push sparkviewengine-1.1.0.0.gem

[![image](/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb10.png)](http://dannydouglass.com/images/2010-08-11-whats-nu-in-the-dotnet-world-part2-creating-a-gem/image_thumb10.png)

**That’s all folks!**  

I can now install the [SparkViewEngine gem](http://rubygems.org/gems/sparkviewengine) into my .NET Project using the command:
    
    nu install sparkviewengine

**_Wrapping Up_**

If you have been looking for that opportunity to contribute to the Open Source community, but keep finding yourself saying, ‘I just do not have the time’, then I am happy to say your chance is here.  Go out and create your first gem. You will be surprised out just how easy, errrr **crazy easy** it is to complete.

Here are some of the resources that aided me in creating my first gem:

  * **Rob Reynold’s Blog Post: **[http://devlicio.us/blogs/rob_reynolds/archive/2010/07/16/how-to-gems-and-net.aspx](http://devlicio.us/blogs/rob_reynolds/archive/2010/07/16/how-to-gems-and-net.aspx)
  
  * **Nu Google Group Post:** [http://groups.google.com/group/nu-net/web/gempublishers?pli=1](http://groups.google.com/group/nu-net/web/gempublishers?pli=1)
  
  * **Nu Google Group Thread:** [http://groups.google.com/group/nu-net/browse_thread/thread/3d4a9618412fd2a4](http://groups.google.com/group/nu-net/browse_thread/thread/3d4a9618412fd2a4)