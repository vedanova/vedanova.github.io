---
layout: post
status: publish
published: true
title: Simple script for creating snapshots of an EBS volume and keeping a certain
  number of those
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 129
wordpress_url: http://blog.vedanova.com/?p=129
date: 2010-11-28 10:48:35.000000000 +01:00
categories:
- tech
- aws
tags:
- AWS
- EBS
---
Thought I share a simple script that creates a snapshot of an AWS EBS volume (for backup purposes) and manages the deletion of old ones. You can specify the number of snapshots you want to keep.

<script src="https://gist.github.com/718805.js?file=AWS%20-%20EBS%20Snapshot%20Management"></script>
