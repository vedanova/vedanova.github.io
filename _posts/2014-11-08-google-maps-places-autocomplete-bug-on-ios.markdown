---
layout: post
title:  "Google Maps Places Autocomplete Form not working on iOS devices"
date:   2014-11-24 18:35
categories:
- tech
tags:
- google maps
- ios
published: true
excerpt_short: Google Maps Autocomplete fails on iOS devices
author: Christoph Klocker
---
For the project I am working on I was implementing the Google Maps [places autocomplete](https://developers.google.com/maps/documentation/javascript/examples/places-autocomplete-addressform)
to auto fill the shipping address from the suggestions Google brings up. Implementation was straight forward, all worked fine
on Desktop, Android, but not on iOS. 

So after doing lots of debugging and the first time even did remote debugging through Chrome I figured out that some event handler
was interferring with the suggestion popup. I've actually did not figure out which one it was in the end.


Anyways, here's my solution. It's a bit of a hack as the I couldn't get the event handling working on the not yet existing DOM element, somehow the
jQuery implementation did not work.

Here's my solution

          q.Logger.debug "AddressAutocomplete Init"
          $('#autocomplete').on 'focus', (e) ->
            q.AddressAutocomplete.geolocate()
    
          @$autocomplete = new google.maps.places.Autocomplete(document.getElementById('autocomplete'),{ types: ['geocode'] })
          google.maps.event.addListener @$autocomplete, 'place_changed', () ->
            q.AddressAutocomplete.fillInAddress()
    
          # Hack for iOS devices
          # ugly but couldn't get the event listener working otherwise
          $('#autocomplete').on 'keyup', (e) ->
            $('.pac-container').on 'touchend', (e) ->
              e.stopImmediatePropagation()
    
          return null