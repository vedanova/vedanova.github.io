---
layout: post
status: publish
published: true
title: Post from a non HTTPS page to a HTTPS controller
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 242
wordpress_url: http://blog.vedanova.com/?p=242
date: 2011-09-14 10:23:30.000000000 +02:00
categories:
- rails
tags:
- rails
- https
- form_for
---
Just had the problem of posting to an HTTPS controller from within a normal page using the form_for helper

Here is how I solved it

    form_for [:admin, @property_edit], :url => admin_property_property_edit_url([:admin, @property_edit], :protocol => 'https'), :html => {:id => 'property_edit'}
