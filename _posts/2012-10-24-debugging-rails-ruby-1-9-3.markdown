---
layout: post
status: publish
published: true
title: Debugging Rails with Ruby 1.9.3
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 345
wordpress_url: http://blog.vedanova.com/?p=345
date: 2012-10-24 07:47:30.000000000 +02:00
categories:
- rails
tags:
- debugger
- 1.9.3
---
Probably anyone who has tried to debug on Ruby 1.9.3 has started to google how to fix the error it is causing.

    /ruby_debug.so: undefined symbol: ruby_current_thread

I once again did it today and came again to the same solution as I already did 2 times before. It is always the same. Go download 3 unofficially release ruby-debug and linecache gems. Thats mainly because the maintainer hasn't updated the gems for the last 2!! years.

Today however I found a new gem that is a fork and fixes all the issues and you can just install it without having the hassle of downloading and installing it manually.
Just install the new gem 'debugger'

    gem install debugger

The github repository is <a href="https://github.com/cldwalker/debugger">here</a>
