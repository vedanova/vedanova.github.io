---
layout: post
status: publish
published: true
title: Setup Zeus for Cucumber Environment  1
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 406
wordpress_url: http://blog.vedanova.com/?p=406
date: 2013-10-24 12:12:00.000000000 +02:00
categories:
- rails
- cucumber
tags:
- cucumber
- zeus
---

If you have separate environments for cucumber and test, to have them run in parallel here's how to set up zeus.


Create the config files

    zeus init

Modify zeus.json
    #zeus.json
    {
      "command": "ruby -rubygems -r./custom_plan -eZeus.go",

      "plan": {
        "boot": {
          "default_bundle": {
            "development_environment": {
              "prerake": {"rake": []},
              "runner": ["r"],
              "console": ["c"],
              "server": ["s"],
              "generate": ["g"],
              "destroy": ["d"],
              "dbconsole": []
            },
            "test_environment": {
              "test_helper": {"test": ["rspec", "testrb"]}
            },
            "cucumber_environment": {
              "cucumber_env": {"cucumber": ["cuc"]}
            }
          }
        }
      }
    }
and modify custom_plan.rb

    #custom_plan.rb
    require 'zeus/rails'

    class CustomPlan < Zeus::Rails

      def cucumber_env
        ::Rails.env = ENV['RAILS_ENV'] = 'cucumber'
      end

    end

    Zeus.plan = CustomPlan.new
