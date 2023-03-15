---
title: "Solution to Root Locked Rogers HTC Dream!"
date: "2010-03-21"
categories: 
  - "development"
  - "featured"
  - "hardware"
tags: 
  - "beta"
  - "development"
  - "htc-dream"
  - "rogers"
---

**Huge Update!**

Since writing this, the DroidX team has come up with an awesome rooting method sans JTAG. The bonus is it seems to work on all Android phones, not just the DroidX.

You'll need:

- The DroidX Exploit: [http://droidx.net/forum/attachment.php?attachmentid=1&d=1279820222](http://droidx.net/forum/attachment.php?attachmentid=1&d=1279820222)
- A Signed SPL: [http://sapphire-port-dream.googlecode.com/files/spl-signed.zip](http://sapphire-port-dream.googlecode.com/files/spl-signed.zip)
- A Recovery Image: [http://rapidshare.com/files/387561074/recovery-RA-dream-v1.7.0R-cyan.img](http://rapidshare.com/files/387561074/recovery-RA-dream-v1.7.0R-cyan.img)
- CyanogenMod (5.0.8 for Android 2.1): [http://n0rp.chemlab.org/android/update-cm-5.0.8-DS-signed.zip](http://n0rp.chemlab.org/android/update-cm-5.0.8-DS-signed.zip)
- An EBI1 Signed Radio[http://briancrook.ca/android/cm-ports/bc-5.0.8-ebi1-signed.zip](http://briancrook.ca/android/cm-ports/bc-5.0.8-ebi1-signed.zip)
- The Google Applications: [http://www.mediafire.com/?marqwt53ii0](http://www.mediafire.com/?marqwt53ii0)
- Finally, the Android SDK: [http://dl.google.com/android/android-sdk\_r06-windows.zip](http://dl.google.com/android/android-sdk_r06-windows.zip)

A great guide can be found at [http://androidheadlines.com/2010/07/guide-on-how-to-root-you-htc-dream-to-2-1.html](http://androidheadlines.com/2010/07/guide-on-how-to-root-you-htc-dream-to-2-1.html) for the steps to follow, or hop on over to the XDA Developers thread for up to the minute information and help.

My personal choice is the Android 2.1 ROM as it runs pretty smooth and you get all the goodies that most other phones still crave :) If you're adventurous the 6.0.0RC1 ROM is out for Android 2.2 but when I installed the image it had quite a few issues still with performance. You can install any custom ROM once you've rooted the phone and a guide on how to do that is found at [The CyanogenMod Wiki](http://wiki.cyanogenmod.com/index.php?title=Full_Update_Guide_-_Rogers_Dream_911_Patched). Happy hacking!

**Kept here for those who are curious enough** There is finally hope at last for all those people that were hurt by Rogers Wireless a month ago. Rogers released a new firmware for the HTC Dream for "911 emergency services" problems. It wasn't a major problem with the phone and more a reason to stop people from rooting their phones on their network. We won't get into that and their shady tactics for locking customers in and their horrible customer service, but we will mention that there is hope for all those locked phones!

A new possible workaround has come out that involves connecting to the phone via JTAG to access the debugger of the ARM firmware loader to load a different ROM than what has been deemed the "Perfect SPL" by modders. This new workaround will allow for a change in the memory address used for loading code so that the user can flash in their own Radio, SPL and ultimately Android image. Since this is a hardware hack, it will be nearly impossible for Rogers to make a software fix for this, and will allow people with bricked HTC Dreams and Magics to be able to revive their phones better than ever. A great job to everyone who was working on the team to find a software or hardware hole, and I'm glad that someone did at least (while decompiling I had found nothing software wise, I was getting quite worried).

For those who are looking for a link, check out:

- [http://forum.xda-developers.com/showpost.php?p=5934885&postcount=6](http://forum.xda-developers.com/showpost.php?p=5934885&postcount=6)
- [http://r3nrut.com/?p=1](http://r3nrut.com/?p=1)

for more info on how the JTAG is constructed and what software is needed. Please be careful though! This is still very early in the review and development stage and there are many thing that are left to be uncovered. Proceed at your own risk, but if you're phone is already bricked, you have nothing to lose! So enjoy your now FREE Android phone, as was the original spirit of the device before Rogers got their hands on it! If you find anything new out or any mistakes please let me know, and if you have a success story please share it with everyone!
