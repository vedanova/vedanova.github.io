---
layout: post
status: publish
published: true
title: How to set maximum number of attempts per job using Delayed Job
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 386
wordpress_url: http://blog.vedanova.com/?p=386
date: 2013-03-28 08:15:09.000000000 +01:00
categories:
- rails
tags:
- rails
- delayed_job
---
In my current application I need to have one job not beeing invoked more than once and another job having at least a minimum of 5 retries before it stops.

There's not much in the documentation about this, but there was a patch that made this possible. Simply add a method max_attempts into your job.


    class MyJob < Struct.new(:id)

      def max_attempts
        5
      end

      def perform
      end
    end
