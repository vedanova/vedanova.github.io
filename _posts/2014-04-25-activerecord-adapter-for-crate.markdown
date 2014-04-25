---
layout: post
status: publish
published: true
title: ActiveRecord adapter for Crate database
author: Christoph Klocker
date: 2014-04-25
categories:
- tech
tags:
- ruby
- crate
excerpt_short: Use Crate as database replacement inside Rails
---

The work to expand the Ruby ecosystem for [Crate](http://www.crate.io) continues. After starting the
at the mountain hackathon I've continued to work on providing a Rails adapter for it.

Developing the basic driver was mostly straight forward, the abstract adapter Rails provides makes it pretty easy to implement
a new driver. The only things I've struggled with, was an issue where I couldn't find the record after creating it. Gave me
some real headaches until I reached out to the Crate guys to find out that Crate is _eventually consistent_, meaning that
a table refreshes after 1000ms and a new record can only be found by the primary key before the refresh.

## Installation

I haven't released a gem yet, as there are still things lacking, like updating nested arrays. I also need to implement
a proper hash mappings. Like always these things might be already fixed when you read this.

So to install it in your Rails (or other framework) project simply add the following to your Gemfile

    gem 'activerecord-crate-adapter', github: "crate/activerecord-crate-adapter"

## Usage

Just update your Rails database.yml

    default: &default
      adapter: crate
      host: 127.0.0.1
      port: 4200

As Crate doesn't come with an autoincrement feature make sure you set one by default in your models. I am using
SecureRandom.uuid.

    class Post < ActiveRecord::Base

      before_validation :set_id

      private

      def set_id
        self.id = SecureRandom.uuid
      end

    end

## Contributing

Feel free to submit pull requests or log issues on Github. I try to implement it when I have time.