---
layout: post
status: publish
published: true
title: Unobstrusive javascript with jquery
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 341
wordpress_url: http://blog.vedanova.com/?p=341
date: 2012-08-07 08:49:21.000000000 +02:00
categories:
- Uncategorized
tags:
- jquery
- unobstrusive
---
Unobstrusive JS is the way to go, no question about it. I was just bothered to always write

    $(document).ready(function() {
       // put all your jQuery goodness in here.
     });

so after a while I found out there is a short cut for that

    $(function() {
    // do something on document ready
    });

even better seems to be to do it the <a href="http://stackoverflow.com/questions/5721372/what-purpose-does-generating-a-closure-inside-of-the-document-ready-functio">closure way,</a> to avoid conflicts

    (function($) {
        // Plugin code...
    })(jQuery);
