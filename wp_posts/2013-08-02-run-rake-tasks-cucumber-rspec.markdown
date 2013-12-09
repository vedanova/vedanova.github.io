---
layout: post
status: publish
published: true
title: Run Rake tasks in Cucumber or RSpec
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 402
wordpress_url: http://blog.vedanova.com/?p=402
date: 2013-08-02 09:13:43.000000000 +02:00
categories:
- rails
- cucumber
tags:
- cucumber
- rspec
- rake
---
Quick writedown on how to run Rake tasks in Cucumber or Rspec.
There are a few answers out on Stackoverflow but those didn't exactly fit my needs. Using the setup they provided worked well on running individual scenarios, but failed when you ran 2 scenarios at the same time, and each did invoke 1 or 2 rake tasks.

My final working solution looks like this

    And /^the rake task to remind of expiring subscriptions ran$/ do
      execute_rake('billing.rake', 'billing:remind_user_of_expiring_subscription')
    end

    def execute_rake(file, task)
      require 'rake'
      rake = Rake::Application.new
      Rake.application = rake
      Rake::Task.define_task(:environment)
      load "#{Rails.root}/lib/tasks/#{file}"
      rake[task].invoke
    end

Kudos goes to this answer on Stackoverflow:
<a href="http://stackoverflow.com/questions/1255176/test-rake-tasks/5306013#5306013">Answer</a>
