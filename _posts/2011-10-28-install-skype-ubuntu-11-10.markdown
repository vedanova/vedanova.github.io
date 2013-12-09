---
layout: post
status: publish
published: true
title: Install Skype on Ubuntu 11.10
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 253
wordpress_url: http://blog.vedanova.com/?p=253
date: 2011-10-28 06:29:46.000000000 +02:00
categories:
- ubuntu
tags:
- ubuntu
- skype
- '11.10'
- oneiric
---
Upgraded my system yesterday to 11.10, and Skype stopped working. Downloaded all kind of binaries and packages from Skype and nothing worked.
After a few hours I found the instructions on the Ubuntu Oeneric page to install Skype like this and it works again.

First add the partner repostiory

    sudo add-apt-repository "deb http://archive.canonical.com/ $(lsb_release -sc) partner"

Then install skype

    sudo apt-get update && sudo apt-get install skype


