---
layout: post
status: publish
published: true
title: ! 'Cucumber: Mouse click anywhere on page (like on a map)'
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 369
wordpress_url: http://blog.vedanova.com/?p=369
date: 2012-11-30 13:53:18.000000000 +01:00
categories:
- rails
- cucumber
tags:
- capybara
- cucumber
- map
- mouse click
---
As I am currently working on an application that triggers an event when clicking on a map I needed to implement a test for that.


What I needed was to trigger a mouse click on an exact position on the map.

First you will need to extend cucumber (capybara). Create the click at functionality:

    # features/support/mouse_click.rb
    Capybara::Node::Element.class_eval do
      def click_at(x, y)
        wait_until do
          right = x - (native.size.width / 2)
          top = y - (native.size.height / 2)
          driver.browser.action.move_to(native).move_by(right.to_i, top.to_i).click.perform
        end
      end
    end

Then implement the step
    And /^I click inside the map to trigger the event$/ do
      find("#map").click_at(50, 50)
    end

You might need some additional step to make sure the map is loaded, something like this

    And /^I wait for the map to appear$/ do
      page.should have_xpath("//div[@class='leaflet-map-pane']")
    end

Hopes this helps.
