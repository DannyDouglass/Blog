---
author: DannyDouglass
date: '2010-11-06 13:50:09'
layout: post
slug: simplify-using-xml-data-with-automapper-and-linqtoxml
status: publish
title: Simplify Using Xml Data with AutoMapper and Linq-to-Xml
wordpress_id: '306'
? ''
: - Articles
  - ASP.NET
  - automapper
  - Open Source
---

I recently ran into a scenario at work that required manually consuming several SOAP web services, which I’m sure you can imagine was rather monotonous. A co-worker (Seth Carney) and I tried a few different approaches, but we finally settled on a solution that simplified consumption of the xml and ultimately made the code more testable.  That solution centered around leveraging [AutoMapper](https://github.com/jbogard/AutoMapper), an open source object-object mapping tool, to create a link between the [XElements](http://msdn.microsoft.com/en-us/library/system.xml.linq.xelement.aspx) returned in the SOAP messages and custom contracts we created - in a reusable manner.

I put together a quick demo that shows how you could use the same approach to consume and display the [Twitter Public Timeline](http://api.twitter.com/1/statuses/public_timeline.xml) (using the API’s Xml response type).

**Note**: The source code for the following example can be found on my GitHub page: [https://github.com/DannyDouglass/AutoMapperXmlMappingDemo](https://github.com/DannyDouglass/AutoMapperXmlMappingDemo)

### 1. Getting the Project Setup

After creating a basic MVC3 ([download beta](http://www.asp.net/mvc/mvc3)) project and the associated test project, the first step was to get the AutoMapper package installed.  I have been using [NuGet](http://nuget.codeplex.com/), Microsoft’s recently announced package management system, to install any open source dependencies.  The following command was all that was needed to setup AutoMapper in my MVC3 project (read more about NuGet [here](http://weblogs.asp.net/scottgu/archive/2010/10/06/announcing-nupack-asp-net-mvc-3-beta-and-webmatrix-beta-2.aspx http://weblogs.asp.net/scottgu/archive/2010/10/06/announcing-nupack-asp-net-mvc-3-beta-and-webmatrix-beta-2.aspx http://weblogs.asp.net/scottgu/archive/2010/10/06/announcing-nupack-asp-net-mvc-3-beta-and-webmatrix-beta-2.aspx http://weblogs.asp.net/scottgu/archive/2010/10/06/announcing-nupack-asp-net-mvc-3-beta-and-webmatrix-beta-2.aspx) and [here](http://www.hanselman.com/blog/CategoryView.aspx?category=Nupack)):
    
    PM> add-package AutoMapper

### 2. Creating the Mapping

With AutoMapper installed I’m ready to get started creating the components necessary for the xml-to-object mapping.  The first step is creating a quick contract used in my application to represent the Tweet object:
    
    public interface ITweetContract
    {
        ulong Id { get; set; }
        string Name { get; set; }
        string UserName { get; set; }
        string Body { get; set; }
        string ProfileImageUrl { get; set; }
        string Created { get; set; }
    }

Nothing crazy here – just a simple entity.  These are all fields that are provided in the response from the Twitter API using a different name for some fields.  In simple cases where the source and destination objects have the same name you can setup a map very quickly using this syntax:
    
    Mapper.CreateMap<SourceObj, DestinationObj>();

However, AutoMapper does not support Xml by default I have to specify the fields that I will be mapping.  Using the Fluent API in AutoMapper I’m able to chain my field mappings.  Take a look at one example field mapped in my example – the tweet’s **Body:**

    Mapper.CreateMap<XElement, ITweetContract>()
        .ForMember(
            dest => dest.Body,
            options => options.ResolveUsing<XElementResolver<string>>()
                .FromMember(source => source.Element("text")))

It may look complicated at first, but all that is really happening here is that we are providing details to AutoMapper on what value to use in my source object and how to map it to the destination object’s property.  There is one particular line I would like to focus on in the above **Body** field mapping:

    options => options.ResolveUsing<XElementResolver<ulong>>()
                            .FromMember(source => source.Element("id")))  

The _XElementResolver _is a [custom value resolver](http://automapper.codeplex.com/wikipage?title=Custom%20Value%20Resolvers) that Seth came up with to handle parsing the XmlElement source object to retrieve a strongly-typed value for use in the mapping.  I’ll detail that more in a moment, but before we move on take a look at my full mapping:

    Mapper.CreateMap<XElement, ITweetContract>()
        .ForMember(
            dest => dest.Id,
            options => options.ResolveUsing<XElementResolver<ulong>>()
                .FromMember(source => source.Element("id")))
        .ForMember(
            dest => dest.Name,
            options => options.ResolveUsing<XElementResolver<string>>()
                .FromMember(source => source.Element("user")
                    .Descendants("name").Single()))
        .ForMember(
            dest => dest.UserName,
            options => options.ResolveUsing<XElementResolver<string>>()
                .FromMember(source => source.Element("user")
                    .Descendants("screen_name").Single()))
        .ForMember(
            dest => dest.Body,
            options => options.ResolveUsing<XElementResolver<string>>()
                .FromMember(source => source.Element("text")))
        .ForMember(
            dest => dest.ProfileImageUrl,
            options => options.ResolveUsing<XElementResolver<string>>()
                .FromMember(source => source.Element("user")
                    .Descendants("profile_image_url").Single()))
        .ForMember(
            dest => dest.Created,
            options => options.ResolveUsing<XElementResolver<string>>()
                .FromMember(source => source.Element("created_at")));

### 3. The Generic XElementResolver  

This custom value resolver is the real key that allowed these XElement-to-Contract maps to work in the original solution.  I’ve reused this resolver in this example as we saw above.  This was all that was necessary to create the custom resolver class:
    
    public class XElementResolver<T> : ValueResolver<XElement, T>
    {
        protected override T ResolveCore(XElement source)
        {
            if (source == null || string.IsNullOrEmpty(source.Value))
                return default(T);  
            return (T)Convert.ChangeType(source.Value, typeof(T));
        }
    }

This generic XElementResolver allows use to easily pass the type of the value retrieved in our mapping above.  For example, the following syntax is used to strongly type the value retrieved from the XmlElement in the _Id_ field’s _.ForMember()_ declaration above:
    
    ResolveUsing<XElementResolver<ulong>>()

With my mapping completely configured and instantiated, I’m ready to invoke the Twitter API and leverage AutoMapper to display that latest Public Timeline.

### 4. Putting the Pieces Together

I created a simple class responsible for retrieving the Twitter API response:
    
    public class TwitterTimelineRetriever
    {
        private readonly XDocument _twitterTimelineXml;  
        public TwitterTimelineRetriever()
        {
            _twitterTimelineXml = XDocument
                .Load("http://api.twitter.com/1/statuses/public_timeline.xml");
        }  
        public IEnumerable<ITweetContract> GetPublicTimeline(int numberOfTweets)
        {
            var tweets = _twitterTimelineXml.Descendants("status")
                .Take(numberOfTweets);  
            return tweets.Select(Mapper.Map<XElement, ITweetContract>).ToList();
        }
    }

The _GetPublicTimeline_ method is a simple method returning, you guessed it, the Twitter Public Timeline by leveraging the map we created earlier:
    
    return tweets.Select(Mapper.Map<XElement, ITweetContract>).ToList();

In my MVC3 site’s _HomeController _I can make a quick call to the retrieval method, requesting the last 10 results:
    
    public class HomeController : Controller
    {
        private TwitterTimelineRetriever _twitterTimelineRetriever;  
        public ActionResult Index()
        {
            _twitterTimelineRetriever = new TwitterTimelineRetriever();  
            ViewModel.Message = "Twitter Public Timeline";  
            return View(_twitterTimelineRetriever.GetPublicTimeline(10));
        }
    }

  

And finally, after a little formatting in my [View](https://github.com/DannyDouglass/AutoMapperXmlMappingDemo/blob/master/AutoMapperXmlDemo.Web/Views/Home/Index.cshtml) using the new [Razor](http://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) view engine from Microsoft, I have my public timeline displaying!

[![Screenshot_TwitterAutoMapperXmlMappingDemo](/images/2010-11-06-simplify-using-xml-data-with-automapper-and-linqtoxml/Screenshot_TwitterAutoMapperXmlMappingDemo_thumb.png)](http://dannydouglass.com/images/2010-11-06-simplify-using-xml-data-with-automapper-and-linqtoxml/Screenshot_TwitterAutoMapperXmlMappingDemo_thumb.png)

I know we had a hard time finding a lot of content on how to leverage AutoMapper to map Xml sources, so hopefully this is as helpful to you as it was for us.  As I mentioned before, the source is available on my [GitHub page](https://github.com/DannyDouglass/) (that I am just starting to utilize):

[https://github.com/DannyDouglass/AutoMapperXmlMappingDemo](https://github.com/DannyDouglass/AutoMapperXmlMappingDemo)

I’d love to hear any other ways people are using AutoMapper with an Xml source.  Shoot me an email at [me@DannyDouglass.com](mailto:me@DannyDouglass.com) if you have an alternative solution.

Cheers!

