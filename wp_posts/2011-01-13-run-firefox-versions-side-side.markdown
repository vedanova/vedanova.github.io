---
layout: post
status: publish
published: true
title: How to run different Firefox versions side by side
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 151
wordpress_url: http://blog.vedanova.com/?p=151
date: 2011-01-13 11:30:03.000000000 +01:00
categories:
- tech
- productivity
tags:
- firefox
- side-by-side
- firefox 4
---
Finally I found out how to run different Firefox version side by side, actually by occasion as someone posted it on a Firefox 4 release thread.
I am running FF4 beta for a while now and not really want to switch back. Sometimes however I need to use an extension (Poster) for webdevelopment testing. Poster was not upgraded yet to Firefox 4 so I need to shut down and start FF 3.6 specifically.
Now what you can do is to start Firefox in profile mode and pass <strong>-no-remote </strong>as additional parameter.
<pre>firefox -P -no-remote</pre>
The command above will prompt you with a profile selection dialog. I just added a new profile called 'test' to it and Firefox 3.6 (in my case) will start up.
Next I can run FF 3.6 directly passing the profile name directly
<pre>firefox -P test -no-remote</pre>
and it will start without showing the profile dialog.
