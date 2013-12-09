---
layout: post
status: publish
published: true
title: ! 'Misleading incompatible character encodings: ASCII-8BIT and UTF-8 message'
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 66
wordpress_url: http://blog.vedanova.com/?p=66
date: 2010-09-30 13:28:54.000000000 +02:00
categories:
- tech
- rails
tags:
- rails
- ruby
- pg
- postgres
---
As I have now spent a second time 2 hours to google this error message and messed around figureing out why this happens I have to write it down.

In my situation I tried to import a set of data a got through the Facebook Graph Api using fbgraph. My database is postgres.

I had a second time the read on Yehuda Katz' blog about Ruby 1.9 and its character encoding (great <a href="http://yehudakatz.com/2010/05/05/ruby-1-9-encodings-a-primer-and-the-solution-for-rails/">read</a> btw), however didn't really help me.

After not finding any solution I started to remove some parameters on my insert statement and finally found out, that the problem has nothing to do with encodings, so its completely misleading, but with the length of the Facebook ID. Facebook IDs need a BIGINT column type in the database, not an INTEGER, so I quickly migrated my db to change the type.

below how you do it on Postgres
<pre>change_column :fanpages, :fb_id, :integer, :limit => 8</pre>

