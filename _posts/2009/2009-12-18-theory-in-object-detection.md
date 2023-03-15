---
title: "Theory In Object Detection"
date: "2009-12-18"
categories: 
  - "development"
tags: 
  - "development"
  - "interfaces"
  - "surface-computing"
---

I have been playing with edge detection for a few weeks now, and slowly I'm gathering a solid knowledge base related to how to do object detection. Primarily I have been playing with facial recognition, but my end goal is to be able to track hands, for a virtual table PC, so that when I build my surface computer the adaption of the algorithms is easy. The goal is to use edge detection and to determine the shape of the enclosed object.

The current algorithm that I am building is based on some assumptions:

- The person is feature complete (two eyes, two eyebrows, one nose, one mouth)
- The room is relatively well lit (haven't incorporated luminosity correction for shape detection, need to work on performance)
- There is a notable edge difference between these features (enough for the algorithm to detect edges)

Assuming these things, the edge detection is mounted in a matrix and through various reduction methods, it reduces the amount of noise (isolated, or only sharing an edge with one cell). From these shapes we can determine facial structure based on clusters of a predefined shape.

While this is fun for face tracking purposes, it strays from the principle concept that is trying to be achieved with virtual touch computing. Facial features are relatively easy to track as they are static in context of each other, so predictable patterns even in rotation are possible. Fingers on the other hand are a more dynamic object to track, as spacing, position count are likely to change frequently. For example someone who wants to drag an object around may only hold out one finger and drag the object where they want to move it, while a rotation might use all five fingers.

On a surface such as a touch PC, this is relatively easy through the user of IR LEDs using the aforementioned FTIR (frustrated total internal reflection) methods of light detection, or through the idea of using a set of IR lasers close to the surface with planers to diffuse the beam over the table, but virtually there is lots of noise in the image. The algorithm that I am currently working on tries to filter out the noise through the following methods:

- Identify the individual in the image (or what is visible of them) and filter out the background image to 0's in the matrix
- Process the general form outline, find where on the matrix the general shape of the hands are, and once this is processed identify the digits
- Filter out everything except the digits and return the points which are visible

This sounds easier than it looks, however there are many limitations and issues when it comes to distinguish-ability. Another problem on top of this is light conditions, multiple people in the image and detectability of their shape. This is the area that I am working on now for performance reasons. Regardless, the skills I obtain developing this algorithm will make surface digit detection trivial in nature.

If anyone has any suggestions on how to detect full human motion based on image capture and processing, please let me know. I am always looking for a good algorithm ;)
