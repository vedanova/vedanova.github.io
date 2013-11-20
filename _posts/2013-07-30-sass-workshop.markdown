---
layout: post
title:  "SASS Workshop"
date:   2013-07-30 09:53:25
categories: tech development
tags: sass
published: true
excerpt_short: Middle of July I was invited to hold a workshop on working with SASS for a local company.
author: Christoph Klocker
---

Middle of July I was invited to hold a workshop on working with SASS for a local company. For those of you who don't
know SASS, SASS (Syntactically Awesome Stylesheets) is a scripting language that is compiled into CSS. I've been working now for
about three years with SASS and can highly recommend to use it. Once you are into it, you won't
ever go back.

### Lets dive into it

First of all, there are two different syntaxes, you can either use the original SASS one, or the CSS compatible
SCSS one. I personally prefer the SASS one, but the SCSS one makes it easy to copy and paste existing CSS code.

Lets have a look at the SASS syntax first

    #sass
    // variables
    $default_width: 500px
    $sidebar_width: 150px
    // mixins
    @mixin box($width: 100px)
      width: $width

    .default_box
      +box($default_width)

    //nesting
    .content
      #sidebar
        width: $default_width
      &.wide
        width: $default_width + 20

and now the same in SCSS Syntax

    #scss
    // variables
    $default_width: 500px;
    $sidebar_width: 150px;
    // mixins
    =box($width: 100px) {
     width: $width;
    }
    .default_box {
      @include box($default_width);
    }

    //nesting
    .content {
      #sidebar {
        width: $default_width;
      }
      &.wide {
        width: $default_width + 20;
      }
    }

the main difference are the brackets and the semicolons (what I usually forget, ... I am coding mostly in Ruby and there you hardly use semicolons).

### Main components

#### Variables

SASS allows you to define variables that you can have in one place and reuse and compute new values using them.

    #SASS
    $base-font-size: 1em
    $base-font: Times, serif
    $headline-font: Helvetica, sans-serif
    $headline-color: #999999
    $font-black: #111

    h1
      font-family: $headline-font
      font-size: $base-font-size * 1.5
      color: $headline-color
    h2
      font-family: $headline-font
      font-size: $base-font-size * 1.2
      color: $headline-color
    p

Give variables a meaningful name

    Bad: $blue-1
    Good: $section-header-blue

#### Color Functions

Color functions help you to create a set of colors that are in the same color space and feel like they belong together,
what otherwise can be difficult (if you are a programmer like me and not a designer). The color functions are provided by
[Compass](http://compass-style.org/). Here's an example how to use them.

    // brand colors
    $logo-blue: #333
    $blue: #043076
    $lighter-blue: lighten($blue, 20%)
    $headline-blue: darken($blue, 10%)
    // buttons
    // confirm-button
    $confirm-button-color: $blue
    $confirm-button-top: lighten($confirm-button, 15%)
    $confirm-button-bottom: darken($confirm-button, 10%)

    .confirm-btn
      border: 1px
      border-bottom-color: $confirm-button-bottom
      border-top-color: $confirm-button-top
      background-color: $confirm-button-color


#### Mixins

Mixins are reusable blocks of code, that are written once and can be used everywhere, they take arguments and you can
set defaults.

    #SASS
    =rounded($vert, $horz, $radius: 10px)
      border-#{$vert}-#{$horz}-radius: $radius
      -moz-border-radius-#{$vert}#{$horz}: $radius
      -webkit-border-#{$vert}-#{$horz}-radius: $radius
    $top: 3px
    $left: 3px

    .rounded
      +rounded(top, left)
    .widget
      +rounded(bottom, right)

compiles into

    #CSS
    .rounded {
      border-top-left-radius: 10px;
      -moz-border-radius-topleft: 10px;
      -webkit-border-top-left-radius: 10px;
    }
    .widget {
      border-bottom-right-radius: 10px;
      -moz-border-radius-bottomright: 10px;
      -webkit-border-bottom-right-radius: 10px;
    }


This should give you a quick overview on SASS.

#### Check out my slides below for more

<iframe src="http://slid.es/christophvedanova/sass-compass-sprites/embed" width="576" height="420" scrolling="no" frameborder="0" webkitallowfullscreen="true" mozallowfullscreen="true" allowfullscreen="true"> </iframe>

#### Resources

* [SASS](http://sass-lang.com/)
* [Compass](http://compass-style.org/)

