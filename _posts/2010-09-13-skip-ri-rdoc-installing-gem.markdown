---
layout: post
status: publish
published: true
title: How to skip ri and rdoc when installing a gem
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 57
wordpress_url: http://blog.vedanova.com/?p=57
date: 2010-09-13 15:40:07.000000000 +02:00
categories:
- rails
tags:
- rails
---
About half a year ago I did this procedure at work and totally forgot about how to do it and couldn't find the documentation anymore. So now I just write a quick blogpost.

simply put the line below into ~/.gem/.gemrc
<pre>gem: --no-ri --no-rdoc</pre>
