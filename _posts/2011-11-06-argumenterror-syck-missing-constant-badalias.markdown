---
layout: post
status: publish
published: true
title: ArgumentError (Syck is not missing constant BadAlias!)
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 263
wordpress_url: http://blog.vedanova.com/?p=263
date: 2011-11-06 19:51:03.000000000 +01:00
categories:
- rails
tags:
- rails
- rails3
---
Had this nasty bug after I had the bug below. Looking at the stack trace I saw that geokit is parsing a cookie and somehow this failed. So clearing the cache solved the bug.

To get rid of the bug below, do a 
<pre>
gem update --system
</pre>
<pre>
Uninitialized constant Syck::Syck (NameError)
</pre>
