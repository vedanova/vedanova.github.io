---
layout: post
status: publish
published: true
title: Expire Page from within model in Rails
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 186
wordpress_url: http://blog.vedanova.com/?p=186
date: 2011-03-01 06:13:56.000000000 +01:00
categories:
- rails
tags: []
---
Just had to expire a cached page from within a model, because I have a delayed_job running that updates my model and therefore I need to expire the cached page I generated from within the controller. Its actually pretty simple, but you still need to know what to call.


    ActionController::Base.expire_page(path)

The path can be a named route or you can build it as simple string.

