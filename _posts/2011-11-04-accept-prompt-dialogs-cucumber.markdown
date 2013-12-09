---
layout: post
status: publish
published: true
title: Accept prompt dialogs with Cucumber
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 256
wordpress_url: http://blog.vedanova.com/?p=256
date: 2011-11-04 13:34:45.000000000 +01:00
categories:
- rails
- cucumber
tags:
- rails
- cucumber
- prompt
---
Today I had a javascript prompt where a user could enter the number of rows he wants to export.
A cucumber step I have implemented to supress the dialog looks like this

    Given /^I accept prompt dialogs with "([^"]*)"$/ do |accept, value|
      page.evaluate_script 'window.original_prompt_function = window.prompt;'
      page.evaluate_script "window.prompt = function(msg) { return '#{value}'; }"
    end

If you want to supress a simple confirmation dialog (Cancel / Ok) you can use a step like this

    Given /^I (accept|deny) confirmation dialogs$/ do |accept|
      page.evaluate_script("window.confirm = function() { return #{(accept == 'accept').to_s}; }")
    end

It's important that you use the step BEFORE you invoke the button.
