---
layout: post
status: publish
published: true
title: Check checkbox in Cucumber - Capybara
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 249
wordpress_url: http://blog.vedanova.com/?p=249
date: 2011-10-18 11:52:32.000000000 +02:00
categories:
- rails
tags:
- capybara
- cucumber
---
Just struggled with checking a checkbox in Cucumber using Capybara.


    Given /^I click the checkbox$/ do
       check(find("input[type='checkbox']")[:id])
    end

