---
layout: post
status: publish
published: true
title: Parsing custom date_select in controller
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 171
wordpress_url: http://blog.vedanova.com/?p=171
date: 2011-02-03 09:04:12.000000000 +01:00
categories:
- rails
tags:
- rails
---
I have a custom date_select in my view and want to parse it in the controller to get back a date.

    <%= date_select("post", "begin", :default => @begin, :start_year => 2011, :id => "begin-select") %>
    <%= date_select("post", "end", :default => @end, :start_year => 2011) %>

this is how you do it in the controller

    @begin = Date.civil(params[:post][:"begin(1i)"].to_i,params[:post][:"begin(2i)"].to_i,params[:post][:"begin(3i)"].to_i) || 7.days.ago
    @end = Date.civil(params[:post][:"end(1i)"].to_i,params[:post][:"end(2i)"].to_i,params[:post][:"end(3i)"].to_i) || Time.now
