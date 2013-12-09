---
layout: post
status: publish
published: true
title: Stub a WillPaginate::Collection
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 270
wordpress_url: http://blog.vedanova.com/?p=270
date: 2011-11-21 08:49:46.000000000 +01:00
categories:
- rails
- cucumber
tags:
- will paginate
---
Today I had to stub a WillPaginate::Collection for my cucumber tests. It's pretty simple.

All you need to do is to call ".paginate" on a collection (Array).
So for me it was as easy as

    Property.should_receive(:paginate).and_return([@property].paginate)

