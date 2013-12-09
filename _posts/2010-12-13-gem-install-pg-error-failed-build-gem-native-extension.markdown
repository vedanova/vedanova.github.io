---
layout: post
status: publish
published: true
title: ! 'gem install pg - ERROR: Failed to build gem native extension'
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 134
wordpress_url: http://blog.vedanova.com/?p=134
date: 2010-12-13 14:37:39.000000000 +01:00
categories:
- tech
- rails
- ubuntu
tags: []
---
Once again I ran into this issue after updateing my gems using 'bundle update' on my Kubuntu 10.10 system.
after a bit of browsing I came across a post that suggested to install some additional libraries.
Installing these libs solved the problem

    sudo apt-get install libpq5 libpq-dev

