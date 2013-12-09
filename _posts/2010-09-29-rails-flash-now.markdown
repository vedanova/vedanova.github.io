---
layout: post
status: publish
published: true
title: Rails Flash.now
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 62
wordpress_url: http://blog.vedanova.com/?p=62
date: 2010-09-29 13:59:36.000000000 +02:00
categories:
- rails
tags:
- rails
- flash
- redirect_to
---
Rails' Flash is a great way to pass notifications from a controller to a view while redirecting.
I just came accross the Flash.now and couldn't remember when to use Flash.now.

Simply you use

Flash for redirecting:
<pre>flash[:notice] = 'You successfully signed up'</pre>

and Flash.now for rendering immediatly
<pre>flash.now[:notice] = 'You successfully signed up'</pre>

Since Rails 2.3 you can also use the default flash options :alert, :notice and :flash direct inside redirect_to
<pre>
redirect_to post_url(@post), :alert => "Watch it, mister!"
redirect_to post_url(@post), :status=> :found, :notice => "Pay attention to the road"
</pre>

