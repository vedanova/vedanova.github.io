---
layout: post
status: publish
published: true
title: Connect with Facebook on Rails with fbgraph
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 37
wordpress_url: http://blog.vedanova.com/?p=37
date: 2010-09-08 19:15:37.000000000 +02:00
excerpt:
categories:
- rails
tags:
- rails
---
The last days I was working on letting user sign up using Facebook Connect. We are planning to integrate some social features into <a href="http://www.gobreadcrumbs.com" target="_blank">Breadcrumbs</a> so that a user will see the latest tracks of his friends and other stuff thats in the pipeline. I was investigating different solutions that integrate with Facebook. Previously I was working with Facebooker, an awesome plugin that did a great job at the last company I worked at, however it seemed that there was no upgrade yet to the latest Facebook Graph API, so I didn't want to use it. On Github I found 3 different projects that is using the Graph API. I had a quick look at all of these 3 plugins, but in the end I decided to go with <a href="http://github.com/nsanta/fbgraph" target="_blank">fbgraph</a>, mainly because it was updated recently. It also had more than one contributor, but that doesn't really say much in the end, as there are great single developers out there who are doing great jobs.

I am trying to remember all steps I had to do to get this up and running. I hope I don't forget anything. These instructions are tested with Rails 2.3.5.
<h3>Installation</h3>

    gem install fbgraph

Be sure to require it in evironment.rb

   require "fbgraph"

<h3>Up and running</h3>

First create a database migration to add the Facebook user id and access token to the users table.

    script/generate AddUserFB

and add this code

      def self.up
        add_column :users, :fb_user_id, :integer, :limit =&gt; 8
        add_column :users, :fb_access_token, :string
      end

      def self.down
        remove_column :users, :fb_user_id
        remove_column :users, :fb_access_token
      end


Next create a controller that handles the authentication.

    script/generate controller fb_oauth


In the controller add following code

  def start
   redirect_to client.authorization.authorize_url(:redirect_uri => "http://request.env['HTTP_HOST']/fb_oauth/callback/" ,
      :client_id => FACEBOOK_SETTINGS['app_id'],:scope => 'email')
  end

  def callback
    @access_token = client.authorization.process_callback(params[:code], :redirect_uri =&gt; "http://request.env['HTTP_HOST']/fb_oauth/callback/")
    session[:access_token] = @access_token
    @fb_user = client.selection.me.info!
  end

Start action is invoked first. This redirects the user to Facebook where the user gets the permission prompt, to allow your application to access the users information. The client_id is your application's ID, the redirect_uri is the URL that is called after the user (hopefully) allows your app to access his information. Scope defines what information you want to access. Less is more ;).

Callback is the action that is called after the authorization. The access_token is used later for interacting with Facebook.

In the application controller we define the client like this

    protected

    def client
      @client ||= FBGraph::Client.new(:client_id =&gt; FACEBOOK_SETTINGS['api_key'],
        :secret_id =&gt; FACEBOOK_SETTINGS["secret"] ,
        :token =&gt; session[:access_token])
    end

In the routes.rb we add following routes

    map.fb_login '/fb_oauth/start', :controller =&gt; 'fb_oauth', :action =&gt; "start"
    map.fb_callback '/fb_oauth/callback', :controller =&gt; 'fb_oauth', :action =&gt; "callback"

Last but not least I created a form /views/fb_oauth/callback.html.haml that is shown to the user to sign up and that posts it to the standard restful_authentication controller.

    #container
    %h2 Sign up to register and connect accounts
    %hr
        %h3= "Hi " + @fb_user.first_name + " " +  @fb_user.last_name
    - form_tag '/users' do
        %fieldset
            %legend Please enter your information below
            %dl
                %dt
                    = label_tag 'Login name'
                %dd
                    = text_field_tag 'user[login]','', :size =&gt; 30
                    %span{:id =&gt; :message}
            %dl
                %dt
                    = label_tag 'email'
                %dd
                    = text_field_tag 'user[email]', @fb_user.email, :size =&gt; 30
            %dl
                %dt
                    = label_tag 'password'
                %dd
                    = "#{password_field_tag 'user[password]','', :size =&gt; 30} (minimum of 6 characters required)"

            %dl
                %dt
                    = label_tag 'confirm password'
                %dd
                    = password_field_tag 'user[password_confirmation]','', :size =&gt; 30
            %dl
                %dt
                    = hidden_field_tag 'user[has_accepted_toc]', '0'
                %dd
                    = check_box 'user', 'has_accepted_toc'
                    = label_tag "I have read and understood the #{link_to  'terms of condition', '/terms-of-condition' , :popup =&gt; ['Terms of condition - Breadcrumbs', 'height=300,width=600']}"
            %dl
                %dt
                    = hidden_field_tag 'user[first_name]', @fb_user.first_name
                    = hidden_field_tag 'user[last_name]', @fb_user.last_name
                    = hidden_field_tag 'user[fb_user_id]', @fb_user.id
                    = hidden_field_tag 'user[fb_access_token]', @access_token
                    = submit_tag 'Sign up'

If everything works you should have the full authentication workflow going if you start up

    http://localhost:3000/fb_oauth/start

Hope this helps
