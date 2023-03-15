---
title: "Get a Reaction With Catalyst"
date: "2010-07-20"
categories: 
  - "development"
  - "featured"
tags: 
  - "applications"
  - "catalyst"
  - "mvc"
  - "perl"
---

[![](images/catalyst_170pix.png "catalyst_170pix")](http://deanpearce.net/wp-content/uploads/2010/07/catalyst_170pix.png)

Hi all, it's been a while since I've updated my blog, but with jobs and projects and plain old laziness it's hard to keep up with everything. For months now I've been playing with Catalyst at work, Ruby on Rails for development and back in the day some CakePHP to feed my PHP addiction. Also I've played with Java using Tomcat to produce web apps at previous jobs, and for the fun of it trying out different technologies. I've finally decided to make use of this wasteful time and put it together into a series of blog posts!

Today, I will be discussing Catalyst which is an MVC web application framework written in Perl. Free to use, just [search CPAN](http://search.cpan.org) for Catalyst for more information. If you're adventurous and know Perl, go ahead and load CPAN and use "install Task::Catalyst" or on the Catalyst page, Matt Trout has posted a wonderful script to speed up installation. I'll cover that part in a bit, but let's say it takes longer than a coffee break. There will be a code link at the **bottom** of the article where you can download my 35 minute Catalyst application to play with!

**What I will be covering:**

- The process from start to finish creating a new shell of a Catalyst application
- A tiny bit about the plugins for Catalyst and which ones I found useful
- Framing up your first application using HTML::Mason
- How to properly enjoy the fact that from start to finish, you have a paginating blog with a decent login system that you can post to in under 30 minutes (after Catalyst initial install)

**The Installation**

Here it gets a bit technical, but I'll explain it some as I go. I've cut out a lot of the processing text for sanity's sake (denoted by " ..." at the end of a line). Besides, if you're serious/already have done the install, who needs to see the auto-generated text anyways :))

**Creating the Database**

This is the backbone of the application, our database. Now, I'm still addicted to PHP like crack, so I still love my PHP tools like PHPMyAdmin. Wonderfully easy to get databases up and running. Organize a database for yourself, I setup a very simplistic (mostly self explanatory) set of tables: _Users (id, username, password, email) and Entries (id, title, body, posted)_. It's best to do your database architecture before we get started as we will be using it right away for model generation. The models can be regenerated at any time, but I recommend getting your database right the first time and then to tweak as needed. First we need a database:

- mysql> create database MyBlog;
- Query OK, 1 row affected (0.37 sec)
- mysql> exit

**Creating the App**

App creation couldn't be easier with Catalyst (basically the same as Ruby on Rails and CakePHP). You give the generator script the name of your app and off it goes creating the scaffolding you need to get your app going. It's important to note that on some operating systems the call is simply "catalyst" instead of "catalyst.pl" (Windows if I am not mistaken, but then who's using that for development anyways :))

- pearce@marvin:~$ catalyst.pl MyBlog
- created "MyBlog" ...

**Configuring the App**

After running this, you will need to follow what the screen tells you and run the Makefile. It will take care of getting everything up and running, but not chmod'ing the scripts to be executable strangely enough. Just chmod the .pl files in the scripts directory that was generated.

- Change to application directory and Run "perl Makefile.PL" to make sure your install is complete
- pearce@marvin:~$ cd MyBlog
- pearce@marvin:~/MyBlog$ perl Makefile.PL
- include /home/pearce/MyBlog/inc/Module/Install.pm ...
- pearce@marvin:~/MyBlog$ chmod +x script/\*.pl

**The Meat and Potatoes: MVC Implementation**

Here is where Catalyst shines in my opinion. It has nice, clean and clear steps for creating the Model, View and Controller. All functionality can be found in the script/myblog\_create.pl script, and you can consult it for further functionality. We are going for speed, so let's create the components as fast as possible. We want a model that comes from our MySQL database without any additional coding, a basic Blog controller (that we can use instead of Root) and we want to use HTML::Mason for the templating. This is done with 3 easy commands as shown.

**Model**

Don't forget to change the "root" and "\*\*\*\*" to your username and password for your database!

> - pearce@marvin:~/MyBlog$ script/myblog\_create.pl model MyBlog DBIC::Schema MyBlog::SchemaClass create=static dbi:mysql:MyBlog root \*\*\*\*
> - exists "/home/pearce/MyBlog/script/../lib/MyBlog/Model" ...

**Controller**

> - pearce@marvin:~/MyBlog$ script/myblog\_create.pl controller Blog
> - exists "/home/pearce/MyBlog/script/../lib/MyBlog/Controller"
> - exists "/home/pearce/MyBlog/script/../t"
> - created "/home/pearce/MyBlog/script/../lib/MyBlog/Controller/Blog.pm"
> - created "/home/pearce/MyBlog/script/../t/controller\_Blog.t"

**View**

> - pearce@marvin:~/MyBlog$ script/myblog\_create.pl view Mason Mason
> - exists "/home/pearce/MyBlog/script/../lib/MyBlog/View"
> - exists "/home/pearce/MyBlog/script/../t"
> - created "/home/pearce/MyBlog/script/../lib/MyBlog/View/Mason.pm"
> - created "/home/pearce/MyBlog/script/../t/view\_Mason.t"

We are now prepped to run our app! We can fire it up by typing "script/myblog\_server.pl" which will fire up a Catalyst running server on port 3000. So if you navigate to http://localhost:3000 you will be able to see your app up and running! The hard part is over, the next steps will be creating the template files in the root directory and creating the actions. I will go over them broadly but it's a lot of concepts to cover. The CPAN repository has great tutorials and cookbooks, but I'll share with you what I didn't know when I started with Catalyst.

1. In typical fashion, the controller looks for the template file of the same name as the page you are requesting. So /login will look for "login" in the root directory that was generated. To change the template (and keep in typical naming fashion you would want something like "login.mason" or "login.mas" or perhaps more general "action.mason" which has parameters. All depends on your necessities. You can change the template for an action by changing the stash template $c->stash->{template} = "myfile.mason".
2. You can create an autohandler file in the root which is executed before any of the action templating. This allows you to insert a header/footer on every page automatically. Simply include the line "% $m->call\_next;" where appropriate in the file to load the requested template file there.
3. Chaining an action is amazing. If you don't understand chaining, please read up on it elsewhere, its fundamental to most MVC designs, and you won't get very far (sanely anyways) without it. It allows you to pass parameters via URL and handle subpages easily. It's made up of the Chain (to a previous action), the PathPart (the actual action you want to associate with) and the Arguments (0 or more, Capture them if you are going to have subpages after the arguments). So something like "sub login : Chained("/") : PathPart("login") : Args(0)" will result in the action /login going to the sub named login.
4. Mason files are very similar to TT, to ERB to JSP. Similar tags, similar structure, you just need to get used to the <%args> parameter at the top of the file, as well as some interesting syntax. You can embed code right in the page, and while not always best practice, it is a godsend for customization.
5. Always leave -Debug on in MyBlog.pm when developing. It shows errors in detail in the browser, it shows errors in the console. You won't be able to understand what's going on without it at least while learning.

**Final Thoughts**

Hard to believe that this blog post took way longer to write than it did to finish up the demonstration blog application. The application from start (creating the db) to finish (creating the tarball) took 35 minutes. A fairly impressive feat, but hardly unbelievable if you are familiar with Perl and some MVC architecture. Here's a look at some of the pros and cons I faced both in speed developing this app and in general developing with Catalyst:

**Pros**

1. Wickedly fast prototyping, churning out a functional application in minutes.
2. Growing community, quite a large amount of documentation on Catalyst and its default model DBIx.
3. Easy to use chaining functionality (in comparison to Ruby on Rails routes file and others).
4. Intuitive templating (not so much a pro, but a definite make it or break it situation).
5. Integrates well with CPAN modules
6. Has a wide variety of plugins available. The one I make use of is called AuthenCookie which handles all your cookie based authentication for you. This is available on CPAN and needed to run the demo. Explore, people write modules frequently and it might shave minutes or hours of your project, with the good feeling of having a community of eyes looking at it for you, keeping it up to date and speedy.

**Cons**

1. Catalyst itself is a nightmare to install. I mentioned this earlier, but seriously, get the installation script or write your own that presses "Enter" over and over :). It has a good 75 modules to install, more if you're starting from a fresh Perl install. Just make sure you have unzip configured for CPAN, write permissions everywhere and that your build tools are working, run the script and go for lunch. It will take upwards of an hour over a mediocre internet connection to get Catalyst going. The upside is once you have it, it's there forever and easy to update/remove.
2. You have to like Perl. And I don't mean just "yeah me and Perl are good pals" but I mean "..must...have...Perl...to...live" kind of situation. Okay, maybe not that much, but an intimate knowledge of Perl (and its special variables) will save you time and headaches. With DBIx::Class I find myself mapping result sets, or iterating using $\_ quite frequently. Frankly, you don't need much more than that, a basic understanding of hashes and how Perl differentiates between a ref and an "object" (for lack of a conceptually easier term). You can pick it up as you go, and the framework covers most of the basics, but you will want to pick Perl up to get the most out of it.
3. You will need some sort of memory caching service (like memcached) to keep any very large scale (or computationally complex) system running at peak performance. This is more of a scalability issue plus overhead than a design issue. Probably best not to be wasting cycles recomputing a homepage anyways.

So please download the package \[**[MyBlog.tar.gz](../wp-content/uploads/2010/07/MyBlog.tar.gz)**\] and play with the app. It is fully functional, albeit kind of silly in terms of security and design, but it would be a good first effort. You get to see templating, parameters, the stash, DBIx (the model we are using, it is an interesting beast, please read up on CPAN about it). And please leave comments/corrections/suggestions in the comments as usual! And remember this is the first in a series of posts that will discuss Ruby on Rails and PHP, and dive into why Java is terrifying for any basic web application.

**Update:** I have now seen the goodness of GitHub and have started a repository. You can view the code for this and future projects at [http://github.com/deanpearce/MyBlogCatalyst](http://github.com/deanpearce/MyBlogCatalyst "http://github.com/deanpearce/MyBlogCatalyst")!

**Update:** My colleague and code guide Yanick has informed me that I could have saved myself a lot of time by disabling running the tests when building the modules. It makes good sense that we could save some time here, especially if we don't anticipate any issues (such as on a fresh install with core prerequisite packages and programs installed such as build-essential and unzip installed). For this I'm a bit ashamed I didn't think of this while installing, and from initial reinstall tests, goes from a lunch break to grabbing a coffee. Thanks Yanick!
