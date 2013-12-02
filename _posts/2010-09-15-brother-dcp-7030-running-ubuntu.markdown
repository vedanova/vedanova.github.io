---
layout: post
status: publish
published: true
title: Brother DCP-7030 running on Ubuntu
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 60
wordpress_url: http://blog.vedanova.com/?p=60
date: 2010-09-15 10:21:49.000000000 +02:00
categories:
- tech
tags:
- dcp-7030
---
quick notes on getting my new printer up and running on Ubuntu

Create cups directory
<pre>sudo mkdir /usr/share/cups/model</pre>

Download driver here
<pre>http://welcome.solutions.brother.com/bsc/public_s/id/linux/en/download_prn.html#DCP-7030</pre>

Install with
<pre>sudo dpkg -i --force-all FILENAME</pre>

Select the driver in the printer driver dialog.
