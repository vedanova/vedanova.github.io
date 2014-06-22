---
layout: post
title:  "Crate with Rails - Object data type"
date:   2014-06-22 22:41
categories: projects
categories:
- tech
tags:
- ruby
- crate
published: true
excerpt_short: Using Crate objects with Rails
author: Christoph Klocker
---

Another great feature of Crate, besides the already mentioned [Arrays](http://vedanova.com/tech/2014/06/16/crate-using-array-data-type.html)
is the possibility to store [objects](https://crate.io/docs/stable/sql/ddl.html#data-types). In Rails you might already have
used it's #serialize functionality to serialize an object or some key/value data into a column. You can do that with Crate 
as well, however it will be stored as an object with (optionally) a defined structure that you can query after.
If you store an address on a user object you can query that object, or better said it's attribute directly, like 

    select * from users where address['city'] = 'Vancouver' limit 100;

So querying after all users that live in Vancouver would be something you don't really wan't to do with a YAML-serialized object.


# How to use it?

As mentioned above I wanted to reuse existing ActiveRecord functionality and looked at AR#serialize. Serialize allows you to 
define your own serialization mechanism, you just need to create a #dump and a #load method in the to be serialized class. 
To make things easy I created a module that can simply be included to get the proper functionality. All you need is to make
all attributes accessible as instance variables and assign them on initialize (see below).

So a serialized class should look like this:

    require 'active_record/attribute_methods/crate_object'
    
    class Address
      attr_accessor :street, :city, :phones, :zip
    
      include CrateObject
    
      def initialize(opts)
        @street = opts[:street]
        @city = opts[:city]
        @phones = opts[:phones]
        @zip = opts[:zip]
      end
    
    end

Check out CrateObject module if you need to write your own serializer. 
 
Then in your model simply use #serialize to have objects working

      class User < ActiveRecord::Base        
        serialize :address, Address  
      end

Note: I do not plan to support nested objects inside objects.

#### Migrations support for objects

In the migrations you can create an object and specify the object schema behaviour (strict|dynamic|ignored) and it's schema.
    
    t.object :address, object_schema_behaviour: :strict,
                       object_schema: {street: :string, city: :string, phones: {array: :string}, zip: :integer}
      
   