---
layout: post
status: publish
published: true
title: Returning 400 for a MultiJson::Decode error
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 378
wordpress_url: http://blog.vedanova.com/?p=378
date: 2013-01-14 12:38:18.000000000 +01:00
categories:
- rails
tags:
- middleware
- multijson decodeerror
---
I am currently working on an API that allows client to create new records. If a JSON request however contains an invalid JSON response an exception is called an a 500, internal server error is returned to the client.

Thats of course not the desired way because it is a client error and not an actual server error. As I couldn't find a solution for that I created a simple Rack middleware that rescues the exception and returns the parsing error to the client.

Here's my gist:

<script src="https://gist.github.com/4529619.js"></script>

Add it to your Rails app
    # application.rb
    config.middleware.insert_before ActionDispatch::ParamsParser, 'ParamsParserRescue'

