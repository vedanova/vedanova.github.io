---
layout: post
status: publish
published: true
title: How to install Postgis 1.5 on Ubuntu Maverik 10.10
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 91
wordpress_url: http://blog.vedanova.com/?p=91
date: 2010-10-18 12:32:44.000000000 +02:00
excerpt:
categories:
- tech
tags:
- ubuntu
- postgis
---
The easiest way to get Postgis up and running on (K)Ubuntu 10.10 is to use the PPA.


Here the instructions

Add the PPA to your Software Sources
Run Kpackagekit (or Synaptic), go to sources and add an additional source by pasting this

    ppa:ubuntugis/ubuntugis-unstable

Refresh your sources and then search for postgis and install the package “postgresql-8.4-postgis”

or by command line
    sudo aptitude install postgresql-8.4-postgis

another gotcha
If you want to connect to postgis with a custom user and get this error message
psql: FATAL: Ident authentication failed for user “username”

edit /etc/postgresql/8.4/main/pg_hba.conf and comment out line below and add the line below.

    # Database administrative login by UNIX sockets
    #local   all         all                          ident
    local all all md5


