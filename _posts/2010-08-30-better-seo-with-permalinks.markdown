---
layout: post
status: publish
published: true
title: Better SEO with Permalinks
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 16
wordpress_url: http://blog.vedanova.com/?p=16
date: 2010-08-30 17:16:50.000000000 +02:00
categories:
- tech
tags:
- rails
- seo
---
Everyone who reads a little bit of SEO knows that a nice URL will bring you quite a bit up in the page rank on Google (or any other search engine). Of course we want to do the same with our tracks on <a title="Breadcrumbs" href="http://www.gobreadcrumbs.com" target="_blank">Breadcrumbs</a>. Breadcrumbs is a GPS Track Management Software that allows you to manage your GPS tracks and upload your videos and photos. Its like Flickr for GPS Tracks, or IPhoto with Google Earth. Check it out.

In Breadcrumbs we allow user to add their tracks in a given category, like mountain biking, running, skiing, .. you get it. We also give them the option to create a bundle. A bundle is a way to group tracks together. Lets say you go on a skiing trip to Austria, you would call the bundle "Austria 2010". So lets say you have your track called "Day 1", so you would have a tree like this: "Skiing" -&gt; "Austria 2010" -&gt; Day 1.

Every user on Breadcrumbs can share their track on Facebook or send a link as email. Now a non restful page would look like this:

<code>gobreadcrumbs.com?track=2</code>

Not really nice and not helpful either to get your rank up, therefore you would rather have something like

<code>gobreadcrumbs.com/Skiing/Austria-2010/Day1</code>

We go even one step further and put the user name into the URL, but thats more for namespacing than any other reason.

<code>gobreadcrumbs.com/Stewart/Skiing/Austria-2010/Day1</code>

Here is a real example:
<a href="http://app.gobreadcrumbs.com/user/corck/Sightseeing/Bali/Bingin-Beach" target="_blank">http://app.gobreadcrumbs.com/user/corck/Sightseeing/Bali/Bingin-Beach</a>

We first (and at the moment still) were limiting the user when they entered their bundle and track name, but this is actually a bad experience. So I decided to use permalinks and tried to find out how Wordpress is doing it. After a bit of Googling and not finding the regular expression anymore (which I found a few weeks back) I decided to look after a Ruby Gem. Don't reinvent the wheel!

<strong>Permalink_Fu</strong>

After a bit of searching on Rubygems.org I decided to go with permalink_fu, I think originally by famous Technoweenie, but forked it from someone called <a href="http://github.com/goncalossilva">goncalossilva</a>.

Permalink_Fu is really easy to set up, first install it:
<pre><code>./script/plugin install http://svn.techno-weenie.net/projects/plugins/permalink_fu/
</code></pre>
Next create a migration to add the permalink column:
<pre><code>./script/generate migration add_permalink_to_track
</code></pre>
The migration looks sth like this.
<pre><code>add_column :tracks, :permalink, :string
</code></pre>
Now for the model code simply add following. Using scope you tell that a permalink needs to be unique within a bundle but can be identical within different bundles. Permalink has built in support for adding a number like "Austria-2010-1" to handle this.

<pre><code>has_permalink [:name], :scope =&gt; :bundle_id</code></pre>

To update existing records simply do

<pre><code>Track.find(:all).each(&amp;:save)</code></pre>

Permalink_Fu comes with a new finder method you can use

<pre><code>@track = Track.find_by_permalink(params[:track])</code></pre>

Hope this helps. Check it out
