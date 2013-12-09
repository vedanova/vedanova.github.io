---
layout: post
status: publish
published: true
title: Use a Rails partial within a form (form_for)
author: christoph
author_login: christoph
author_email: christoph@vedanova.com
author_url: http://www.gobreadcrumbs.com
wordpress_id: 69
wordpress_url: http://blog.vedanova.com/?p=69
date: 2010-10-06 12:28:40.000000000 +02:00
categories:
- rails
tags: []
---
Currently i am creating a form that changes its content dynamically on switching a select box. To keep my code clean I am creating 3 different partials. As I want to be DRY, I keep the form statement in the main ERB file.
To get this to work you just need to pass in the local variable <strong>f</strong> of the form.

&lt;%= form_for(@model) do |f| %&gt;
<div id="_mcePaste">&lt;p&gt;</div>
<div id="_mcePaste">&lt;%= f.text_field :name %&gt; Please enter a descriptive name</div>
<div id="_mcePaste">&lt;/p&gt;</div>
<div id="_mcePaste">&lt;p&gt;</div>
<div id="_mcePaste">&lt;%= render :partial =&gt; 'form', :locals =&gt; {:f =&gt; f} %&gt;</div>
<div id="_mcePaste">&lt;div&gt;</div>
<div id="_mcePaste">&lt;%= f.submit 'continue' %&gt;</div>
<div id="_mcePaste">&lt;/div&gt;</div>
<div id="_mcePaste">&lt;% end %&gt;</div>
