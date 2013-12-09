---
layout: post
status: publish
published: true
title: Rspec - Stub outgoing requests
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 338
wordpress_url: http://blog.vedanova.com/?p=338
date: 2012-08-02 13:03:53.000000000 +02:00
categories:
- Uncategorized
tags: []
---
If you want to stub outgoing requests (and do not care about the response) use Webmock and stub it like below:

    stub_request(:any, /googleadservices\.com/)

for more examples how to stub the response read the <a href="https://github.com/bblimke/webmock/">documentation</a>
