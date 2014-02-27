---
layout: post
status: publish
published: true
title: How to test on mouse over with capybara and selenium
author: Christoph
date: 2014-02-27
categories:
- tech
tags:
- rails
- capybara
- selenium
excerpt_short: Testing on mouse over with selenium
---

Once again I had to test a mouse interaction and see if the tooltip shows up. Here's the code for capybara and selenium.

    Then(/^I hover over the element it shows the tooltip$/) do
      el = find('#example-btn')
      page.driver.browser.action.move_to(el.native).perform
      page.should have_content "My tooltip content"
    end

We need "el.native" to the the selenium element node and not the capybara one. To access the action builder we need to go
through the browser.
