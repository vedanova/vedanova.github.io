---
layout: post
status: publish
published: true
title: Printer is cutting off top of the page - Brother
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 410
wordpress_url: http://blog.vedanova.com/?p=410
date: 2013-11-05 10:18:54.000000000 +01:00
categories:
- ubuntu
tags:
- dcp-7030
- brother
---

I am using a Brother Printer (DCP-7030) on Linux and have set it up using the drivers from the brother page. See my guide <a href="http://blog.vedanova.com/2010/09/15/brother-dcp-7030-running-ubuntu/">here</a>.
Unfortunately for some reason the top of the page is cut off when printing a page. After trying to set all settings in the printer configurations, nothing worked. After googling around I found a thread that helped me to fix it.

Open up your driver configuration

    sudo nano /usr/local/Brother/inf/br<MODEL>rc

It should something like this

    [DCP7030]
    Language=LANG_USA
    Resolution=600
    PaperSource=Tray1
    Duplex=OFF
    DuplexType=Long
    PaperType=Letter
    Media=PlainPaper
    Copies=1
    Sleep=PrinterDefault
    TonerSaveMode=OFF

Now find the line PaperType and change it to A4 (or whatever you need)

    [DCP7030]
    Language=LANG_USA
    Resolution=600
    PaperSource=Tray1
    Duplex=OFF
    DuplexType=Long
    PaperType=A4
    Media=PlainPaper
    Copies=1
    Sleep=PrinterDefault
    TonerSaveMode=OFF

Now restart cups

    sudo service cups restart