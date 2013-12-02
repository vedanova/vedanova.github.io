---
layout: post
status: publish
published: true
title: How to add a user to Postgres on Ubuntu
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 359
wordpress_url: http://blog.vedanova.com/?p=359
date: 2012-10-25 19:19:19.000000000 +02:00
categories:
- Uncategorized
tags:
- postgres
---
Quick writeup on how to create a new user on postgresql
<pre>
sudo -u postgres createuser --superuser $USER
sudo -u postgres psql
postgres=# \password $USER
</pre>
