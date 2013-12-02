---
layout: post
status: publish
published: true
title: Return geojson by default using Rgeo
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 365
wordpress_url: http://blog.vedanova.com/?p=365
date: 2012-11-05 12:13:41.000000000 +01:00
categories:
- rails
tags:
- rgeo
---
RGeo by default renders spatial columns in WKT. To switch it to GeoJSON, set this in an initializer:

<pre>
RGeo::ActiveRecord::GeometryMixin.set_json_generator(:geojson)
</pre>
