---
title: "Edge Detection Using C#"
date: "2009-12-13"
categories:
  - "development"
tags:
  - "development"
  - "interfaces"
---

Whoa, it has been way too long since I have been on here, but I am coming back with some interesting new projects. I have spent many months working at the Pythian Group as a Perl software developer. Fantastic experience in my opinion, but anyways, I have been experimenting in my free time with using C# for various projects. I want to use it primarily because it is an easy and efficient language, but secondarily to help improve my skills.

I have been playing with my Neural Network software, and with the idea of starting a company. Basically I got my software to a state where it can parse data from text based sources into the neural network and firing a query would highlight related items down the neural path. I am going to continue playing with this idea as I think it has potential in specialized searching and data recollection niches.

The new toy project I am working on is Edge detection and cleaning in C#. Now I know there are like a billion projects out there that play with these ideas, but I would like to be able to play with it myself. I have assembled a working demo of this project, though the source is not quite ready to release yet for everyone's eyes. Currently it uses a simple color edge detection algorithm, as that sort of algorithm will plug in nicely with my image signature detection algorithm that I was developing a few months back. Basically all it can do now is edge detection in real time, which is a feat for me, I have been away from the specialized programming for several months now, but am slowly getting back into it.

\[caption id="attachment\_87" align="alignleft" width="320" caption="Edge Detection Algorithm at Work"\][![Edge Detection Algorithm at Work](images/Image3.jpg "Image3")](http://deanpearce.net/wp-content/uploads/2009/12/Image3.jpg)\[/caption\]

This is a sample shot from an early version of my algorithm. It detects edge colour differences in a given webcam image and currently overlays the original bitmap. While this traditionally works quite well, it tends to produce a lot of noise and wide edges. For this reason, I'm working on a newer algorithm that slices the image into zones, which then calculate the noise ratio independently and adjust the thresholds for the edge detection appropriately. Currently the changes are looking promising but need to undergo a large amount of changes before I will be able to rely on it for all my data processing.

The next step is to detect "object" edges, which are complete logical elements in images (house for example, window, etc) and process them into sub-object "textures" to be stored and signed using my other algorithms. This will allow the camera to begin to categorize and classify items in the real world, for what will be unveiled as my larger scale project in future posts. Hopefully sometime soon I will get a chance to clean the source and upload an example of my edge detection algorithms using the zone sampling method.

I am now finishing up my co-op term, meaning that I will have a lot more time now to play with programming, so expect more posts in the coming weeks and months!
