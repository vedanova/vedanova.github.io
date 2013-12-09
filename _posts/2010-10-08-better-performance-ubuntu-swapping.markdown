---
layout: post
status: publish
published: true
title: Better performance in Ubuntu with less swapping
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 77
wordpress_url: http://blog.vedanova.com/?p=77
date: 2010-10-08 06:47:23.000000000 +02:00
categories:
- ubuntu
tags:
- ubuntu
- swap
---
A lot of my time I spend on programming for <a title="Breadcrumbs" href="http://www.gobreadcrumbs.com" target="_blank">Breadcrumbs</a>. Breadcrumbs is based upon Google Earth plugin, which is still!! not running on Linux, and I guess it never will, they probably will move it to WebGl and have it cross platform. So finally it will be possible having it on Linux but probably not for another year.

Anyways, for testing I had to have my Virtualbox running all the time, to do some testing and the VM is eating quite a lot of RAM. So most of the time I had Netbeans, multiple browsers, the rails server, and lots of other stuff running. So it quickly got up to 2,5 Gb of RAM. Everything worked smooth up to 2.1 GB, then Ubuntu started swapping (even if I had 3GB of RAM) and as my harddisk is not the fastest (something to look at in future) the system got slow. Sometimes it happened that Netbeans started reindexing and then it really fucked up. I could sometimes go for a coffee until I could use it again.

I started wondering why Ubuntu started swapping that early, as I still had 900 MB left. So I started posting on threads but no really solution (quite a surprise) and somehow never came to a solution. Now a few days I realized, that I was playing around with the 'swappiness' setting about two years back, so I wondered if that would solve my problem. The kernel swappiness settings defines at what level it starts swapping memory to disk, and Ubuntu sets it to 60, that means it starts swapping when 40% of RAM is used. So thats quite a low setting, but what I read it makes sense in server environments. Most people however recommend using a setting of 10. So I tried it a few days setting it manually.

just do this to set it manually
<pre>echo 10 &gt; /proc/sys/vm/swappiness</pre>
if you think this is better than before, after testing a few days, at least what I did. you can put it into /etc/sysctl.conf so its by default.
<pre>sudo nano /etc/sysctl.conf</pre>
add at the end of the file
<pre>vm.swappiness=10</pre>
hope this helps you as much as it did me
more information can also be found at <a href="https://help.ubuntu.com/community/SwapFaq#What is swappiness and how do I change it?">help.ubuntu</a>
