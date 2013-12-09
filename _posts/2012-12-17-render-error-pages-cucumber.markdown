---
layout: post
status: publish
published: true
title: Render error pages in Cucumber 1
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 374
wordpress_url: http://blog.vedanova.com/?p=374
date: 2012-12-17 08:34:42.000000000 +01:00
categories:
- rails
- cucumber
tags:
- error-pages
---
Today I was confronted with testing my API in Cucumber (I should actually write a post on it). The scenario I was testing was involving deleting a resource that does not belong to this user. As the resource it is not found it raises a ActiveRecord::RecordNotFound error, what is actually ok.
The API client should actually not see the error, but have a 404 returned. I've implemented a custom error handling using an error controller and use the routing middlewhere as the "exceptions_app". More on this can be found <a href="http://stackoverflow.com/questions/10253366/need-to-return-json-formatted-404-error-in-rails">here</a>.

Back to my original problem. Everything was working fine. Testing the destroyment using curl was giving me a nice 404 and a "error- not found" JSON response. However Cucumber was raising the error and did not show the error page. I spent some time googling and found a work around. Trying to implement that workaround in env.rb I saw a comment in the env.rb

    # By default, any exception happening in your Rails application will bubble up
    # to Cucumber so that your scenario will fail. This is a different from how
    # your application behaves in the production environment, where an error page will
    # be rendered instead.
    #
    # Sometimes we want to override this default behaviour and allow Rails to rescue
    # exceptions and display an error page (just like when the app is running in production).
    # Typical scenarios where you want to do this is when you test your error pages.
    # There are two ways to allow Rails to rescue exceptions:
    #
    # 1) Tag your scenario (or feature) with @allow-rescue
    #
    # 2) Set the value below to true. Beware that doing this globally is not
    # recommended as it will mask a lot of errors for you!</blockquote>

So rendering error pages is built in in Cucumber (should have thought about that earlier). It's as simple as tagging the scenario with <strong>@allow-rescue</strong>.
