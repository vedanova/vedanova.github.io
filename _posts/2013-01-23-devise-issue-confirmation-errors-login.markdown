---
layout: post
status: publish
published: true
title: Devise issue with confirmation errors when having a login name
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 381
wordpress_url: http://blog.vedanova.com/?p=381
date: 2013-01-23 09:51:40.000000000 +01:00
categories:
- rails
tags: []
---
Just opened up an issue pointing out a bug that is related to a devise change allowing either to use a login or user name. If you try to resend the confirmation message and a user is alread confirmed the user did not see the error message.

<a href="https://github.com/plataformatec/devise/issues/2236">Issue on github</a>
