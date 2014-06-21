---
layout: post
title:  "Crate with Rails - Array data type"
date:   2014-06-16 22:30
categories: projects
categories:
- tech
tags:
- ruby
- crate
published: true
excerpt_short: Crate Rails with array columns
author: Christoph Klocker
---

One feature of [Crate](http://crate.io) is the support of various data types. Today I was working to get the Array type
working in the [ActiveRecord adapter](https://github.com/crate/activerecord-crate-adapter) for Crate. 

# When would you want to use arrays?

A common scenario for arrays is when you have tags for an article or a recipe. Each article can have one or more tags and 
you usually need to create a new "tags" table for it (or you serialize it). Allowing you to have a native tags column saves
you from adding additional complexity you might not want. No new table, no model, no indexes.... just a new column that
you can save your tag array straight to it.

# How to use it?

Migrations now support it by using t.array and passing array_type

     t.array :tags, array_type: :string
     t.array :votes, array_type: :integer
     t.array :bool_arr, array_type: :boolean
     
Creating an object is as simply as

    Post.create!(title: 'Arrays are awesome', tags: %w(hot fresh), votes: [1,2])
    
... and querying for it

    post = Post.where("'hot' = ANY (tags)")    
    

