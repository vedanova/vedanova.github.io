---
layout: post
status: publish
published: true
title: How to fix Cucumber Unresponsive Script errors
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 396
wordpress_url: http://blog.vedanova.com/?p=396
date: 2013-04-09 08:35:00.000000000 +02:00
categories:
- rails
- cucumber
tags: []
---
I was just dealing again with "unresponsive script" errors in cucumber when I was testing if an error message appears on the page.
As always I used

    page.should have_content "Error message"

and then Firefox got unresponsive and threw the error. I've even upped the default waiting time in the FF preferences (user.js), max_wait_timeout. However didn't work for me either.

I now found a fix for this in using a different selector. I am going for XPath now and all is working fine.

    page.should have_xpath("//*[text()='#{text}']", :visible => true)


Wonder if I should default to XPath.
