---
layout: post
status: publish
published: true
title: Send notifications from Android to your desktop with Android Notifier
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 82
wordpress_url: http://blog.vedanova.com/?p=82
date: 2010-10-13 13:11:58.000000000 +02:00
categories:
- android
- apps
tags:
- android
- apps
---
Just came across <a title="Android Notifier" href="http://code.google.com/p/android-notifier/" target="_blank">Android Notifier</a>, a nice little app that sends notifications from your Android to your Desktop. Nice thing, it works cross platform with Linux, Mac and Windows.

What does it do? Android Notifier sends notifications like new SMS / MMS, low battery, and even notifications from Third parties to your desktop. Especially nice is, that its working through your WIFI, so you don't have to enable Bluetooth (which sucks battery) for this to work (but you can opt for Bluetooth if you like).

So to set it up just follow the instructions on the wiki page or do like this on (K)Ubuntu

Add this to your /etc/apt/sources.list

    deb http://android-notifier-desktop.googlecode.com/svn/deb-repo stable main

    sudo apt-get update
    sudo apt-get install android-notifier-desktop

Start the Android desktop notifier.
to get Wifi working I had to open the port

    sudo ufw allow 10600/udp

Try it out using the 'Send test notification' on the Android App
