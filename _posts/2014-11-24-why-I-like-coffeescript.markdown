---
layout: post
title:  "Why I like Coffeescript"
date:   2014-11-24 08:35
categories:
- tech
tags:
- Coffeescript
- Javascript
published: true
excerpt_short: Why I like Coffeescript
author: Christoph Klocker
---
Currently I am writing more and more CoffeeScript. First I was not too impressed by it, due to the fact that it brings not much
more to the table than syntactical sugar for JavaScript. I had a few discussions weather or not TypeScript is better, mainly
because it already uses ECMAScript 6 functionality and extends on it. However, now after some more time coding CoffeeScript I really
start to like it, because the main advantage is readability, and coming from Ruby, readability is what makes your code beautiful,
understandable and maintainable (besides having a decent test suite).

A quick example that should express the readability:

    # CoffeeScript
    return unless target? && MutationObserver?

returns into

    # JavaScript
    if (!((target != null) && (typeof  MutationObserver !== "undefined" &&  MutationObserver !== null))) {
      return;
    }