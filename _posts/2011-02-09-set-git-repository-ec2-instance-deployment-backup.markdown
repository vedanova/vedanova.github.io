---
layout: post
status: publish
published: true
title: Set up a Git repository on an EC2 instance for deployment and backup
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 177
wordpress_url: http://blog.vedanova.com/?p=177
date: 2011-02-09 17:04:42.000000000 +01:00
categories:
- tech
- rails
- aws
tags:
- rails
- AWS
- ec2
- capistrano
---
As I am just setting up a new EC2 instance for my latest project I am going to set up a remote Git repository (as I am not using Github) . Always good to have a back up in the cloud, currently I have a daily cron job that syncs my local to my dropbox folder as well.

What do you need to do

<strong>Setup the Repository</strong>

1. Login to your server and init a git repo

    cd
    mkdir project.git
    cd git/
    git --bare init

2. Logout and add it to your remote repos. Add your public key to the EC2 server, so you don't have to type in the password all the time

    git remote add origin ssh://app@domain.com/~/project.git

3. ready to push

    git push origin master

So you should have a nice backup on your EC2 instance.

<strong>Deployment using this repository</strong>
Next we set up capistrano to use this repository for deployment
1. First we need to allow your user on the remote server to log into the server, yup thats right.
so log in to the server and do a

    ssh app@domain.com

say yes to add it permanently to your known hosts. you might have to add your public key to the authorized keys.
2. Check that your /mnt/app/.... has the proper permissions of this user. When I did a deploy:setup it belonged to the user root
3. update your capistrano deploy.rb

    set :gitdomain, "domain.com"
    set :gituser, "app"
    set :repository, "#{gituser}@#{gitdomain}:project.git"
    set :scm, :git
    set :deploy_via, :export

4. do the deployment (of course you need to set up the other config params I dont mention here, like location....

    cap deploy


