---
layout: post
title: "Synergey Key Mapping Between Mac and Windows"
date: 2011-10-03 21:49
comments: true
categories:  [osx, mac, windows]
---

#My Brain is going to Explode!

Synergy is doing a great job about sharing keyboard and mouse between OSes. But when it comes to mac and windows, there are some new problems.

Mac and Windows have different keyboard shortcut. The most confusing shortcut fighting in my head is copy and paste. Synergy support copy and paste between machine which is nice. So I can copy some text from windows and paste them in mac.

Here comes the problem. I control-c in my windows and go to my Mac I hit control-v all the time. This is a vice versa problem. To solve it, I need to map control key to command key.

But setting up keybinding is hard in synergy. It is quite hard to make synergy working between my Mac Lion and Win 7. So I decided not to touch the config file of synergy.

#Solution: KeyRemap4MacBook

Download from tekezo website: [http://pqrs.org/macosx/keyremap4macbook/index.html](http://pqrs.org/macosx/keyremap4macbook/index.html)

Direct Link : [http://pqrs.org/macosx/keyremap4macbook/files/KeyRemap4MacBook-7.4.0.pkg.zip](http://pqrs.org/macosx/keyremap4macbook/files/KeyRemap4MacBook-7.4.0.pkg.zip)

After installing the pkg and reboot, go to System Perferences->KeyRemap4MacBook.

Tick the following combobox:

-  Shift_L to Lazy_Shift_L
-  Shift_R to Lazy_Shift_R
-  Option + Tab to Command + Tab
-  Use PC Style Copy/Paste
-  Use PC Style Undo
-  Use PC Style Select All
-  Use PC Style Save
-  Use PC Style Find
-  Use PC Style New Tab in Browser

#Done
Now I can use control-c and control-v in both of my machine. The shortcut won't mess up my head anymore. :)
