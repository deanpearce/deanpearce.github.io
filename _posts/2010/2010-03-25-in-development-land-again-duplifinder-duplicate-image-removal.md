---
title: "In Development Land Again: DupliFinder, Duplicate Image Removal"
date: "2010-03-25"
categories: 
  - "development"
tags: 
  - "beta"
  - "development"
  - "interfaces"
  - "project-news"
  - "search"
---

Interesting enough, the more stressed I become the more I desire to program. And not just random programming, but programming to solve a problem (namely one of me :)). What I've been working on over the past couple days is a nice Windows utility to work with and manage duplicate files. Right now it works on the head of the file (64KB) or entire file for comparison, but future versions (such as the one I'm working on now) are going to include audio stream matching (to compare audio regardless of the format or compression) and video frame matching along with a host of other options. Here's a screen shot of it in action:

\[caption id="attachment\_104" align="aligncenter" width="573" caption="DupliFinder Duplicate File Finder"\][![DupliFinder Duplicate File Finder](images/UI-feature-complete-1024x623.png "UI-feature-complete")](http://deanpearce.net/wp-content/uploads/2010/03/UI-feature-complete.png)\[/caption\]

As you can see the UI is almost complete, allowing you to select a root folder and you can scan current and sub directories optionally. The only problem with it at the moment is it's single-threaded, so when it is given a large folder to process it feels as if the program has frozen, but don't worry it's crunching away in the background.

The feature I'm adding right now is a preview/compare pane for easy verification that the file is the same, as well as giving the user to option to delete the "duplicate" or "original" in case of file confusion. This will be ready to go for the v1.0 release of the utility.

If anyone is interested, I can package up a preview version that has some quirks and the single-threaded problem. I had looked all over for a program that would allow you to delete duplicate images and other files and it's ridiculous the amount of money those scam-like software companies try and charge for such simple (but effective) software! Anyways I'm contributing back to the community by releasing this under GPLv2. On top of handling photos (for which I've seen some software cost $30US!) it will handle all sorts of stream and data related information.

For any programmers out there reading this, it was programmed using Visual C# 2008 on a Windows 7 development box, so all sorts of shiny Windows 7 features may start peeking through soon :).

If you have any comments, please feel free to leave them below, and if you would like to play with the source or the compiled version of this app, just let me know in the comments and I will fire off some more information and a package. That's all for now!
