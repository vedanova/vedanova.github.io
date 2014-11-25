---
layout: post
title:  "Scripting Nginx with MRuby"
date:   2014-10-15 20:35
categories: projects
categories:
- tech
- open source
tags:
- ruby
- mruby
- nginx
published: false
excerpt_short: 
author: Christoph Klocker
---

This week there was another web developer meetup and @chaudum was presenting NGINX scripting with Lua. I really liked his
talk as it demonstrated some usecases that could be quite useful in future. 

# Setup

    # http://cubicdaiya.github.io/mruby_nginx_module/

    $ git clone https://github.com/cubicdaiya/mruby_nginx_module.git
    $ cd mruby_nginx_module
    $ git submodule update --init
    $ cd mruby
    $ # Needed for redis    
    $ rake ENABLE_GEMS="true" CFLAGS="-O2 -fPIC"
    
    # Nginx
    $ wget http://nginx.org/download/nginx-1.7.6.tar.gz
    $ tar -xvzf nginx-1.7.6.tar.gz
    $ cd nginx-1.7.6 
    $ ./configure --add-module=../mruby_nginx_module --with-http_ssl_module
    $ make
    $ sudo make install


    # extend build_config
    $ cd mruby_nginx_module
    $ nano build_config.rb
    
    # add additional gems
    #
      # Recommended for ngx_mruby
      #
      conf.gem :github => 'iij/mruby-io'
      conf.gem :github => 'iij/mruby-env'
      conf.gem :github => 'iij/mruby-dir'
      conf.gem :github => 'iij/mruby-process'
      conf.gem :github => 'iij/mruby-pack'
      conf.gem :github => 'iij/mruby-digest'
      conf.gem :github => 'mattn/mruby-json'
      conf.gem :github => 'matsumoto-r/mruby-redis'
      conf.gem :github => 'matsumoto-r/mruby-vedis'
      #conf.gem :github => 'matsumoto-r/mruby-memcached'
      conf.gem :github => 'matsumoto-r/mruby-sleep'
      conf.gem :github => 'matsumoto-r/mruby-userdata'
      conf.gem :github => 'matsumoto-r/mruby-uname'
      conf.gem :github => 'mattn/mruby-onig-regexp'
      conf.gem :github => 'ksss/mruby-file-stat'
      
    # edit nginx.conf to include custom configuration
    include /home/ckDev/work/mruby-nginx/conf.d/*.conf;
    
    # create configuration
    $ mkdir conf.d
    $ nano mynginx.conf

Lets create a simple endpoint to test

    server {

       listen 8000;
       root /home/ckDev/work/mruby-nginx/nginx_root;

       location = /ping {
         access_log logs/pings.log;
         return 201;
       }
    }
    
Start the server and send a request, it should return a 201 
    
    $ /usr/local/sbin/nginx
    $ curl localhost:8000/ping -I
        HTTP/1.1 201 Created
        Server: nginx/1.7.6
        Date: Wed, 15 Oct 2014 19:03:32 GMT
        Content-Type: application/octet-stream
        Content-Length: 0
        Connection: keep-alive

Lets get some mruby going and lets create a first hello world example     

    # -- hello world example
    location = /hello_world {
        mruby_content_handler_code "
            r = Nginx::Request.new
            r.content_type = 'text/plain'
            Nginx.rputs('Hello, World!\n')
        ";
    }

    $ curl  localhost:8000/hello_world
        Hello, World!

Lets assign some variable we can use

    location ~ /user/(.*)$ {

        set $user $1;

        mruby_content_handler_code "
            r = Nginx::Request.new
            r.content_type = 'text/plain'
            Nginx.rputs(r.var.user)
        ";
    }
    
    $ curl localhost:8000/user/corck
        corck

Lets move the code into its own script

    location ~ /user/(.*)$ {
    
    	set $user $1;
        mruby_content_handler "/home/ckDev/work/mruby-nginx/conf.d/mruby.rb";
    }    
    
    $ curl localhost:8000/user/corck
         corck
         
         
Lets manipulate the response header and add an expire header
         
    location ~ /user/(.*)$ {

	    set $user $1;

        mruby_header_filter_code "
          time      = Nginx::Time.time()
          http_time = Nginx::Time.http_time(time + 60 * 60 * 24)
    
          hout            = Nginx::Headers_out.new
          hout['Expires'] = http_time.to_s        
        ";
    }
    
    $ curl localhost:8000/user/corck -I
        HTTP/1.1 200 OK
        Server: nginx/1.7.6
        Date: Wed, 15 Oct 2014 20:49:10 GMT
        Content-Type: text/plain
        Content-Length: 5
        Connection: keep-alive
        Expires: Thu, 16 Oct 2014 20:49:10 GMT

    