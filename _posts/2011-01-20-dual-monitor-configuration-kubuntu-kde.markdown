---
layout: post
status: publish
published: true
title: Dual monitor configuration on Kubuntu (KDE)
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 161
wordpress_url: http://blog.vedanova.com/?p=161
date: 2011-01-20 18:04:11.000000000 +01:00
categories:
- ubuntu
tags:
- kubuntu
- kde
- linux
---
I think I have been using Linux permanently since 9 years. Lots of things have improved since then, I can remember that I had to reboot my Laptop if I hadn't plugged in my mouse at startup. I remeber I had to create some hot- or coldplug scripts later, so it worked while it was running. Anyways, one of the things still is not working is a simply plug n play of an additional monitor, at least on KDE. KDE inludes a Dual Monitor Configuration settings panel, however it doesn't recognise my second monitor when I plug it in (or I can't set it up as a scond, can't remember really anymore).
Xrandr is a great tool that comes preinstalled on Kubuntu, that allows to switch on monitors / or monitor ports easily and also recognizes if a monitor is connected.
Below is a simple script that checks if a monitor is connected, if its connected it extends the desktop and puts the notebook monitor (LVDS1) left of the connected monitor (VGA1). If you use the script check if your monitors are called VGA or VGA1, same for LVDS.

On KDE login I simply run this script, (just link to it from .kde/Autostart). If I disconnect the monitor I run it manually. As said, this actually sucks, would be good if this would be solved, Win98 could do it better than Linux, and that is 13!! years ago.

    #!/bin/bash
    myvar="$(xrandr -q)"
    if [[ $myvar == *"VGA1 connected"* ]]
    then
      xrandr --output VGA1 --auto;
      xrandr --output LVDS1 --auto;
      xrandr --output LVDS1 --auto --left-of VGA1;
    else
      xrandr --output VGA1 --off;
      xrandr --output LVDS1 --auto;
    fi
