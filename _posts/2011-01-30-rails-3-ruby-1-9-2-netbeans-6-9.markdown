---
layout: post
status: publish
published: true
title: Rails 3 and Ruby 1.9.2 with Netbeans 6.9
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 167
wordpress_url: http://blog.vedanova.com/?p=167
date: 2011-01-30 18:13:44.000000000 +01:00
categories:
- rails
tags:
- rails
- netbeans
---
If you switch to Rails 3 and Ruby 1.9.2 (or 1.9.1) you will get the error

    Uncaught exception: no such file to load -- script/server

a simle edit of rvm/gems/ruby-1.9.2-p0/gems/ruby-debug-ide19-0.4.12/lib/ruby-debug-ide.rb will help

Replace the line 123

    bt = debug_load(Debugger::PROG_SCRIPT, options.stop, options.load_mode)

with the following code:

    if Debugger::PROG_SCRIPT.match(/script\/server/)
    p_script = './script/server'
    else
    p_script = Debugger::PROG_SCRIPT
    end
    bt = debug_load(p_script, options.stop, options.load_mode)
