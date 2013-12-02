---
layout: post
status: publish
published: true
title: the page you are looking for is temporarily unavailable. please try again later.
  -nginx
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 328
wordpress_url: http://blog.vedanova.com/?p=328
date: 2012-06-08 11:37:29.000000000 +02:00
categories:
- Uncategorized
- tech
tags:
- rails
- nginx
- passenger
---
I've just released an update to Breadcrumbs. After several months of coding we upgraded to Rails 3.1, implemented Community Engine and did some more updates. Everything worked fine locally but of course something has to go wrong on production.

While testing on production I got randomly 
<pre>
the page you are looking for is temporarily unavailable. please try again later. 
</pre>

WTF. so I crawled through the logs and found 
<pre>
 Exception PGError in application (server closed the connection unexpectedly
  This probably means the server terminated abnormally
  before or while processing the request.
</pre>

strangely I did not found that in the production.log (no errors there) but in the nginx_error.log

so after googling a bit I found on <a href="http://stackoverflow.com/questions/7257872/passenger-concurent-connections-error">Stackoverflow</a> the solution. The problem seems to be related to Ruby 1.9.2, Rails 3.1 and PG gem.
The fix was to set another Passenger spawn method ( in nginx.conf)
<pre>passenger_spawn_method conservative</pre>
instead of the default
<pre>passenger_spawn_method conservative smart-lv2</pre>

Hope this helps someone out there. Will need to upgrade my server to 1.9.3 soon, the startup time is lot less, and probably this error won't exist at all.
