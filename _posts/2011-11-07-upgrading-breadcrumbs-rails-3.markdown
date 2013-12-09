---
layout: post
status: publish
published: true
title: Upgrading Breadcrumbs to Rails 3
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 266
wordpress_url: http://blog.vedanova.com/?p=266
date: 2011-11-07 10:59:37.000000000 +01:00
categories:
- rails
tags:
- rails
- rails3
---
Currently I am upgrading my GPS Track Management Platform <a href="http://www.gobreadcrumbs.com">Breadcrumbs</a> from Rails 2.3.9 to Rails 3 and finally to Rails 3.1.
The upgrade went surprisingly well (I am still not finished) but after a doing the little things that Ryan is showing in his
Railscast <a href="http://railscasts.com/episodes/225-upgrading-to-rails-3-part-1" target="_blank">Rails 3 upgrade</a>, my app was working again. Most of my work was upgrading the ActiveRecord finders and scopes to the new syntax.

The most cumbersome part I experince is upgrading my specs from rspec1 to rspec2. All the route_for has changed to route_to. Hopefully when that is done I can progress on to doing in-depth testing.
