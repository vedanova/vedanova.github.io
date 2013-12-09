---
layout: post
status: publish
published: true
title: Xpath's preceding and following in JQuery
author: admin
author_login: admin
author_email: you@yourdomain.com
wordpress_id: 232
wordpress_url: http://blog.vedanova.com/?p=232
date: 2011-07-21 07:18:24.000000000 +02:00
categories:
- tech
tags:
- jquery
- xpath
- preceding
- following
---
just had to fight with the problem of selecting a preceding element in JQuery. JQuery comes already with the prev() and prevAll() selectors that select the previous elements if they are direct siblings.
My problem was now that I had following structure.

<pre> 
&lt;form&gt;
  &lt;input /&gt;
  &lt;span&gt;
     &lt;input id="2" value="Ineed2know" /&gt;
  &lt;/span&gt;
  &lt;input id="3" /&gt;
&lt;/form&gt;
</pre>

Now my problem was, that I needed the value of input#2 beeing on input#3. So it would have been the preceding input, except that it was nested inside a span. So prev() and prevAll() won't work.

What I really wanted is the XPath's :preceding selector. So I came up with this extension:
<pre>// find a preceding element inside a parent element, omitting the dom structure
jQuery.fn.preceding = function(parentSelector, elementTypeSelector) {
   var inputs = this.closest(parentSelector).find(elementTypeSelector);
   return $(inputs[inputs.index(this)-1]);
};</pre>
so what you do is to pass in a selector that selects one of the parent elements, up to you and then the selector for the JQuery.find().

in aboves example it would look like this:
<pre>  $(this).preceding('form', 'input').val();</pre>
Of course you can have more specialized selectors like "input.className"

here the snippet for 'following'

<pre>
 // find a following element inside a parent element, omitting the dom structure
jQuery.fn.following = function(parentSelector, elementTypeSelector) {
   var inputs = this.closest(parentSelector).find(elementTypeSelector);
   return $(inputs[inputs.index(this)-1]);
};
</pre>
