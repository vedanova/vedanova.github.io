---
layout: post
status: publish
published: true
title: Chrome / Chromium is calling controller twice
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 110
wordpress_url: http://blog.vedanova.com/?p=110
date: 2010-11-24 09:26:57.000000000 +01:00
categories:
- tech
- rails
tags:
- rails
- chrome
- webkit
---
Another hour spent on debugging a strange behavior of Chrome (Chromium), might even be a Webkit problem. When I was loading a page, Chrome loaded the page twice, I relized that when having a break point in my Rails controller and saw it stopped there twice.

First I thought it might be a routing problem or some of my plugins are somehow interfering or its one of my before_filters. So I started commenting out my before_filters, and investigated if this is controller specific or all over the place. And yes, it was all over the place.

Not finding a solution I started googling and found this post on StackOverflow: <a href="http://stackoverflow.com/questions/2009092/page-loads-twice-in-google-chrome">page loads twice in Google chrome</a>.
So, first of all I checked if this was a Chromium specific issue, and yes it was. Firefox did not show this behaviour. The post talked about an empty img src tag that caused it. After commenting out some code I found my bug, it was not an empty img src tag, but an empty stylesheet reference

&lt;pre&gt;&lt;link rel="stylesheet" href="" type="text/css"/&gt;﻿&lt;/pre&gt;

So be warned, never have an empty reference in your html files!
