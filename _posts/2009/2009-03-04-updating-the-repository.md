---
title: "Updating the Repository"
date: "2009-03-04"
categories: 
  - "development"
  - "site-updates"
---

After nearly a month absence, I return! Well, not really gone as much as out of sight. I've been busy preparing for and writing midterms, and doing assignments and travelling. Boston near the beginning of my disappearance, home for reading week and out and about with friends. Now that I've got the travel bug out of my system, I'm back and ready to develop. First thing I've done since my return was update the repository. It now functions a lot better, and looks cleaner and prettier. Hopefully I can get the kinks worked out of it, but I do know of the issue where it's not accessible from repository.deanpearce.net, so I've changed the link (and you should change yours too) to point to deanpearce.net/repository. For some reason it rends fine when pointed to from that directory. Probably some strange little error in my script, but I think it looks pretty good for a few hours of work and hacking together code. It also allows for inline code previews, line numbering and syntax highlighting, so its easier to click through the source. I will be adding a zip file to each directory so you can download the source as a zip package, and I will be including project files and makefiles so the code can be built and tested with ease.

And sometime in the next day or so, I will be putting v0.2 of the Game Engine online! This version adds fixed animation support, more packet options, a simple HUD (heads up display), double buffering and (if you want to play with it) the ability to change play area sizes. The SmoothAnimation class didnt make the cut for this release of the game unfortunately, but there is a sample MusicPlayer class built on some FOSS Java MP3 libraries which makes its debut. I will be working to release SmoothAnimation and the completed MusicPlayer classes sometime this month, then I will unfortunately be slowing work on the Java version of the game in favour of working in C# with DirectX. This will allow for a complete client and server rewrite, and I will be using a new network model and graphics model for the game. It will be exciting, but it will also depend on my free time. Here's to hoping!

In other news, if anyone wants an FTP account for the repository, please just email me or drop a comment below with a way to contact you and I will provide up to 250MB of storage space for your source code that you want to be publically visibile. Happy coding!
