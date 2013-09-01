---
author: DannyDouglass
date: '2011-02-24 10:02:54'
layout: post
slug: create-a-recursive-ruby-script-to-replace-file-and-directory-names-and-file-contents
status: publish
title: 'Creating a Recursive Ruby Script to replace File/Directory Names and File Contents'
---

Over this past weekend I began templating a project structure that we use at my [day/evening/night job](http://dannydouglass.com/aboutme/) to allow developers to simply execute a script and provide a few parameters to
initialize a new development project (including build script, fxcop analysis, unit testing, visual studio projects, etc.).  I didn’t think this would take that long despite being relatively new to the Ruby world.

Oddly enough, I had trouble finding similar examples on the [interwebs](http://www.urbandictionary.com/define.php?term=interwebs) – even StackOverflow didn’t have the answer!  [The horror, the horror](http://en.wikipedia.org/wiki/Hea rt_of_Darkness)!

No matter, I’m a developer after all and <strike>should be</strike>, _better be _capable of handling this simple script.   <!-- more -->I decided to start simple and just use the directory global searching method to recursively get everything within my project template’s directory:
    
{% gist 6402016 %}

OK, good start.  Now I have all the items (files and directories) within my current executing directory.  I figured it would be easy to then replace the templated text in the directory and file paths at that point.

**Not so fast!**

The global directory search - _Dir.glob()_ – actually traverses the directory structure and returns an array of all the file/directory names within the executing directory.  Take a look at the ruby documentation on the it [here](http://ruby-doc.org/core/classes/Dir.html).  What ended up happening was that as I changed directory names using a **top-down** approach, I was
invalidating paths used later in the loop.  For example, _/project/store_ is the first directory to change in the example structure below:

**Original Directory Structure**   

+ /project/store/  - [*first to change*]  
+ /project/store/hats  
+ /project/store/jackets  

When we try to change _/project/store/hats_ ruby throws an error because the root path _/project/store_ would have changed.  The fix for this was simple,reverse the array and go from **bottom-up**:
    
{% gist 6402038 %}

Better.  Time to add the logic to replace the placeholders I put in the file and directory names.  At this point I don’t care about whether the item is a file or directory so I treat them the same:
    
{% gist 6402046 %}

Much better.  Now I need to determine if I am dealing with a file or a directory since I will need to replace the contents of files where I find the placeholder text as well.  Turns out that was pretty darn simple.

#### Final Script
    
{% gist 6402052 %} 
    
I can think of a lot of ways to improve this for more complicated scenarios, but this worked just fine for my needs.  Hopefully this helps someone else too.  And as I mentioned previously, I’m relatively new to Ruby and welcome and constructive feedback or suggestions for improvement that any Ruby experts have for me.