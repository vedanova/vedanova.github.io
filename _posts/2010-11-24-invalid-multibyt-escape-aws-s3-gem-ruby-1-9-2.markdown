---
layout: post
status: publish
published: true
title: invalid multibyte escape using aws-s3 gem and ruby 1.9.2
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 115
wordpress_url: http://blog.vedanova.com/?p=115
date: 2010-11-24 10:58:42.000000000 +01:00
categories:
- tech
- rails
tags:
- rails
- s3
---
If you get following error using the aws-s3 gem (I am using it with paperclip to serve my images from S3)
<pre>gems/aws-s3-0.6.2/lib/aws/s3/extensions.rb:84: invalid multibyte escape: /[\x80-\xFF]/</pre>
Add the following as the first line of <code>lib/aws/s3/extensions.rb</code>:
<pre># encoding: BINARY</pre>
