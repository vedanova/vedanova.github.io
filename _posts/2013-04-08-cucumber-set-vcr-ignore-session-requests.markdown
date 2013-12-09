---
layout: post
status: publish
published: true
title: Cucumber - Set up VCR to ignore session requests
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 392
wordpress_url: http://blog.vedanova.com/?p=392
date: 2013-04-08 13:18:20.000000000 +02:00
categories:
- cucumber
tags:
- vcr
---
When running VCR for a cucumber selenium test it will record by default every request. Unfortunately every button click results in a Selenium URI request, and each uri has a unique session key that will change for each run, so you end up having a file of 9000 lines for a single run and 18000 for the 2nd run.

Fortunately VCR comes with a simple ignore port configuration so you can ignore these requests.


    VCR.configure do |c|
      c.hook_into :webmock
      c.cassette_library_dir = File.expand_path('../../cassettes', __FILE__)
      c.allow_http_connections_when_no_cassette = true
      c.ignore_request do |request|
        URI(request.uri).port == 7055
      end
    end
