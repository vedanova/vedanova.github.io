---
layout: post
status: publish
published: true
title: Livereload - a webdevelopers little friend
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 142
wordpress_url: http://blog.vedanova.com/?p=142
date: 2011-01-12 08:52:08.000000000 +01:00
categories:
- tools
- productivity
- rails
tags:
- rails
- productivity
---
Some time ago I discovered a small little helper called Livereload. Its a little ruby gem and chrome/chromium extension that saves you a reload of a page when editing changes. CSS and JS changes are injected even without reloading the page, other changes, like erb changes are refreshed by reloading the page.

From the github description:

LiveReload is a Safari/Chrome extension + a command-line tool that:

Applies CSS and JavaScript file changes without reloading a page.
Automatically reloads a page when any other file changes (html, image, server-side script, etc).

Installations is as easy as
<pre>sudo gem install rb-inotify livereload</pre>
more information is found on the official <a href="http://https://github.com/mockko/livereload">Github repo</a>


Greg Pollock has done a nice screencast on it:


<object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" width="480" height="385" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="allowFullScreen" value="true" /><param name="allowscriptaccess" value="always" /><param name="src" value="http://www.youtube.com/v/EZ8vy_cNMVQ?fs=1&amp;hl=de_DE" /><param name="allowfullscreen" value="true" /><embed type="application/x-shockwave-flash" width="480" height="385" src="http://www.youtube.com/v/EZ8vy_cNMVQ?fs=1&amp;hl=de_DE" allowscriptaccess="always" allowfullscreen="true"></embed></object>
