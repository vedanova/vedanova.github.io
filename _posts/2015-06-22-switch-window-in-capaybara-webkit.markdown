---
layout: post
title:  "Switch window in capybara webkit"
date:   2015-06-22 11:50
categories:
- other
tags:
- rails
- selenium
- capybara-webkit
published: true
excerpt_short: Switch windows in capybara webkit
author: Christoph Klocker

---
I was just trying to get an automated test for Amazon Payment checkouts to put in place, however I failed in the end
because Amazon is protecting (for a good reason) automated checkouts. I even failed when entering the captcha in the test
run to record it once using VCR.

However it took me a little while to figure out how to switch the windows as its slightly different implemented than in the
 Phantom JS driver and there are no good reads on it on the web, so hopefully it will be helpful to other people.
 There are 2 ways to achieve it.

### Switch driver to new window

    #switch the focus
    page.driver.browser.window_focus page.windows.last.handle
    ... do whatever you want

### Execute an action or inspect the content of the page then continue on main window

    page.driver.within_window page.windows.last.handle do
      fill_in "email", with:  "myemail@mailinator.com"
      find(".signin-button").click
    end

### Inspect window handles hash

    page.driver.browser.window_handles
    => [
     [0] "{16262221-1070-4036-b4a8-f1d7af2d6dd8}",
     [1] "{f37d0424-67c6-4e78-8a5f-77da37e27805}"
    ]


