---
layout: post
status: publish
published: true
title: Open page after failed step - Cucumber
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 361
wordpress_url: http://blog.vedanova.com/?p=361
date: 2012-10-30 07:35:44.000000000 +01:00
categories:
- rails
- cucumber
tags: []
---
If you are running a cucumber scenario and it fails most of the time you need to inspect what's wrong and you want to look at the page why it does not work as expected.

Usually you would add the step

   Then show me the page

Then run it again and look at the page. Wouldn't it be better to open the page right away after a failing step?

Just add following snippet in an existing step, I have it in a new file support/open_on_failure_steps.rb

    After do |scenario|
      if scenario.status == :failed
        save_and_open_page
      end
    end

and it will automatically open the failed page for you.
