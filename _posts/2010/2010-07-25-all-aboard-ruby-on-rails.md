---
title: "All Aboard Ruby On Rails"
date: "2010-07-25"
categories: 
  - "development"
  - "featured"
tags: 
  - "applications"
  - "mvc"
  - "rails"
  - "ruby"
---

[![Ruby On Rails](images/ruby_on_rails_logo.jpg "ruby_on_rails_logo")](/wp-content/uploads/2010/07/ruby_on_rails_logo.jpg)

Last night I posted the code for the next blog installment which is using the Ruby on Rails web framework for building your blog application. You can find this demo code in the new GitHub repository at [http://github.com/deanpearce/MyBlogRails](http://github.com/deanpearce/MyBlogRails "http://github.com/deanpearce/MyBlogRails"). And please do look at it, play with it, and tell me what I've done wrong as usual :). I have spent time over the past few weeks familiarizing myself with Ruby and how its gems, such as Rails, extend the functionality of the language. For those of you who are new to Ruby on Rails, it's important to understand that Rails is a Ruby gem, meaning that the gem extends the language to make Ruby a friendly language for web development. As per usual, I am assuming that you are caught up in at least the fundamentals of the Ruby programming language, and have followed the installation guide (for Linux as usual :)) at [http://wiki.rubyonrails.org/getting-started/installation/linux](http://wiki.rubyonrails.org/getting-started/installation/linux "http://wiki.rubyonrails.org/getting-started/installation/linux"). The process is rather straightforward, and even easier on Ubuntu as most (if not all) the packages can be installed using Aptitude to get the development process going without needing to wait for builds.

Overall, this iteration of the project got easier, since the HTML::Mason and Rails ERB views are very similar in construction. It became an exercise in substitute the correct language code while keeping existing logic in place. For my own sanity, I added a users controller so I can control accounts through a web UI once you are authenticated (hint: this will be a lead up to a potential second round, integrating user accounts and external services such as Gravatar :P). Compensating for the time I spent in developing the basic template for Catalyst (a grand total of 3 of the 35 minutes), it works out that from scratch, development of a blog application took about 20 minutes in Ruby on Rails, which is stunning! But that isn't without a few pitfalls that I will go over later in the article.

**What I will be covering:**

- Generating your first Rails Application
- Framing the structure using scaffolding
- What comes built-in to Rails (and why the way I used sessions isn't the brightest)

**The Installation Process**

This section is pretty short, and overall I have no complaints! Using Aptitude to install on Ubuntu, the process from opening the Wiki to having the default "You're Riding On Rails" page was maybe 10 minutes, significantly faster than Catalyst. And because of the way we will be playing with Ruby on Rails, we won't be doing our database creation! Well, we will be using a MySQL database, but we will use the _rake_ tool to handle the complex parts of the database creation and generation for us.

**Creating the App**

Whoa! We have already cut a lot of the time out from the Catalyst application. The install process was a few simple apt-gets (in Ubuntu, a few simple makes everyone else) and we can skip the database creation phase! This is great for us since we are trying to shave minutes off development. In the real world, one would want to plan all of this out beforehand, such as planning your controllers rather than your database, but for our purposes we will do this ad-hoc. Switch to your favourite development directory and type:

- pearce@marvin:~/Projects$ rails -d MyBlogOnRails
- create ...

Great, the application was generated! You might be wondering what the -d flag is. By default RoR (Ruby on Rails) uses an SQLite database, the -d flag tells it to build as if we are going to use a MySQL database. The flag may be -d or -D depending on the source you install and version, so keep an eye out for that. Very similar to Catalyst, and as we'll discover, pretty much the same structure all the popular web frameworks use. Now that we have an application, let's start creating with it.

**Configuring the App**

The first thing is we need to do is give the app some credentials for the database. Ruby on Rails differentiates between development, testing and production. The names are pretty self-explanatory and for our needs we will be using the generated default of development. Switch to your project directory MyBlogOnRails and edit config/databases.yml:

- pearce@marvin:~/Projects$ cd MyBlogOnRails
- pearce@marvin:~/Projects/MyBlogOnRails$ vi config/database.yml

_development:  
adapter: mysql  
database: myblog\_development  
username: root  
password: \*\*\*\*  
host: localhost_

This will tell RoR that our database, which we will call myblog\_development, can use those credentials at that host. There are sections for production and testing also in the file, and you can edit those at your leisure, but for our purposes we will only be using development. That's it for setup, nothing to make or build or chmod. Let's continue on to Grand Central: the implementation of the model, view and controller.

**Grand Central: MVC Implementation**

If so far your RoR experience has been good, this is where things may switch tracks for some or go express for others. There are some tips and tricks that are necessary for a smooth experience that can be read in the [Ruby on Rails Wiki](http://wiki.rubyonrails.org/ "Rails Wiki") such as naming convention of controllers and types to specify when generating the controller and such. If you have this knowledge in your head, Rails will turn your development express. If you're like me and don't like reading documentation before you start hacking around (though I always end up reading it, and improving what I do :)) then this might be the thing that derails your project (I'm on a roll with these puns :D). The big holdups for me were the following:

- When using the generator name your controllers in the singular (it will automatically handle making things plural as needed), else you will encounter "dependency missing" message hell.
- The documentation on what is going on when things go wrong at this point is _**horrible**_! Nine out of ten sources (sites, docs, comments) say "oh well regenerate your app" and that's it!
- For in-depth projects you will want to plan your database either independently and write your own controllers, or plan your controllers (down to the order things will be created).

Once you get over that, it's fairly smooth sailing. First attempts at it were causing premature manual baldness and I am really surprised that for such a large and vibrant community these basic problems aren't explained more often (and in ways that are easy to search for on a search engine).

**Controller**

So now let's generate the controllers that we will need for this project:

- pearce@marvin:~/Projects/MyBlogOnRails$ ruby script/generate scaffold post title:string body:text
- pearce@marvin:~/Projects/MyBlogOnRails$ ruby script/generate scaffold user username:string password:string email:string

At first this may look a little confusing, why are we telling our controller what we need to include, and what is this scaffolding? This will be explained after we finish generating everything :)

And that's it for generating code! What, I bet you thought it was going to be more. Scaffolding is a cool tool included in Rails to generate most of the code needed to get a full MVC application up and running. It creates the model, view files and the controllers all in a nice setup for you to hack away on. For the advanced developer, or very custom components it isn't too helpful, but for beginners its a great way to see a tangible application, right now with not one line of code! We really are slowly coding ourselves out of jobs aren't we :P. If you are going to be generating the model on your own, you can add the parameter _\--skip-migrations_ to the generate script, which will still generate all the code for you, but running db:migrate will not attempt to create the tables that you need. One **important** thing to note is that if you want the default action to be the posts controller, you will need to manually edit the routes.rb file in the config directory _map.root :controller => "post"_ right before the _end_ directive in the file, and you will need to delete the default index.html.erb template. This will ensure the root action is the post controller and that you won't accidentally run it with the index.html.erb file.

The next big step will be to actually create the database and tables for our application, and now that our model is defined, we can use the rake tool included in rails:

- pearce@marvin:~/Projects/MyBlogOnRails$ rake db:create
- pearce@marvin:~/Projects/MyBlogOnRails$ rake db:migrate

And if we've given the correct permissions to our database user we should get messages saying it has created the database, and a message for each table it creates. As of now, we're done! Well, not yet :) but if you want to see your RoR app, you can run:

- pearce@marvin:~/Projects/MyBlogOnRails$ script/server

Which will start a default Mongrel server on port 3000. Navigate to this and you will get the scaffold generated interface! This is all so exciting, a fully running app that you can technically "post" to with no code at all. But as you can see, no security or style either. Let's take it to the next step and look at sessions and creating a controller for logging in:

- pearce@marvin:~/Projects/MyBlogOnRails$ ruby script/generate controller user login

This creates our new controller along with the login action/views. Notice we are not doing scaffolding here as the implementation will be done in pure manual code and can be found in the GitHub repository under the controller and views of the same name. In principal we want to do the same thing as the Catalyst version: only allow posting when logged in and a basic login/logout system that just gives us access to the new post page and logout button.

The big helper here is the built-in session management provided by Rails. This means no fiddling around with authentication like in Catalyst, we can immediately use the session call from within our application like _`<%=session[``:username``]%>.`_ This is exactly how it was implemented in the blog application, and while it seems good at first, you realize that we depend on the session not being forged and solely on the username :P. In a real system this might not fly as reasonable authentication, but for our purposes it'll be like Fort Knox. Just as easy as we can set a session variable using _session\[:key\] = val_ we can use _reset\_session_ to erase our session. I won't go into code detail as I'm assuming you know at least a tiny bit about Ruby, or you have checked out the project and investigated the controller\_user.rb file in app/controllers. Once you rework your templates using Ruby syntax such as _@post.each do |post|_ and setup your session management you will be good to go.

**Final Thoughts**

Once again, it amazes me that it took far long to write (and find time and motivation for :P) writing the blog about the application than it did to actually write it. Ruby on Rails does one thing and does it exceedingly well: turns ideas into applications quickly with minimal code. Like any framework though, once you get more complicated than the basic single table layouts and non-scaffold actions things get a bit more complicated. Ruby on Rails sped things up by allowing me to skip figuring out where to place the model and the views, and allowed me to skip some basic templating and routing, since this was all automatically generated.

**Pros**

1. If I thought Catalyst was wickedly fast at prototyping, this is like that star being hurled across space at 2.5 million KM/h. Plain and simple, it's fast.
2. Routing (chaining and paths in Catalyst) is cake as long as you don't need to make modifications to the default values. Things get a bit more when you have to edit the routes.rb file, but the advantage is in that file you can see all the actions and controllers and exactly what they do. I like that, but I also like to know what the controller is doing in the actual controller file. So pluses and minuses to each system.
3. Default templating is mind numbingly easy, and since HTML::Mason and the ERB files (along with the templating in Cake and JSP pages) they are all very similar in structure. In fact, with simple language replacement, you could migrate templates across systems with almost no work at all. And the scaffolding in Rails allows me to not even need to know how to make a default template (though I do) but just build on what it generates.
4. Gems are great, I mean Rails, much like Catalyst, is just a module.

**Cons  
**

1. Documentation for anything before actually writing custom controllers, and modules and such is a nightmare, they have great installation guides and how-to walkthroughs online, but anything that sways from the path, or funny errors because you forgot something minor are hard to find documentation for from my experience. Also, the default course of action most people provide is "oh well regenerate your application", which in the case of being like 3 command line entries in isn't bad, but let's say something went awry once much deeper into the project, regenerating/restarting is unthinkable.
2. The gems are a great feature, but the downside is they are harder to find and not often organized into just a repo. Some are built and from Aptitude, some are from individual sites, there are lots on RubyGems.org, but it just doesn't feel as well organized. Maybe I'm just too new to see the elegance of the system, or I missed Gems 101, but it doesn't feel as solid as Perl in that sense.
3. Mongrel is nice but when the default suggestion is to load balance it I start to question performance a bit :) much like Catalyst, memcached plugs into Rails easily with _Cache.put_ and _Cache.get_, so it's probably a good idea to integrate that into your code, and to anticipate using Mongrel Cluster on Apache (or some other httpd) for maximum performance. The upside is this allows for very easy distribution across servers.

So that's it, I might have some updates to the post in future weeks, or after feedback. As usual, leave a comment below, or email me and I will respond to you. I will not be posting packages in my posts anymore in favour of uploading them to GitHub or to my local repository. Next on the list is CakePHP, which I look forward to as PHP is my secret addiction and I want to finish playing with it and put a post together on that. A friend of mine suggested that I add Django, which is a Python web framework to my list of things to try, so that has been added to the plate for after CakePHP!
