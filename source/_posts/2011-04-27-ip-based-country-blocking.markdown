---
author: Soumik Ghosh
date: '2011-04-27 12:20:35'
layout: post
comments: true
slug: ip-based-country-blocking
status: publish
title: IP based country blocking
wordpress_id: '64'
categories:
- General Configuration
tags:
- geoip
- ip
---

To block visitors from a specific country (or countries) using the IP
addresses, Nginx needs to be compiled with the [GeoIP module][1]. This module
was introduced in v0.7.63 and 0.8.6. In addition to that you need a Geo
database. Install these packages under Debian :

	apt-get install geoip-database libgeoip1

We now need to edit the configuration files. Put this code in the `http` block
in `/etc/nginx/nginx.conf`. It can be also put in your vhosts config file
outside of the `server` block.

    
    geoip_country /usr/share/GeoIP/GeoIP.dat;
    map $geoip_country_code $allow_visit {
    	default yes;
    	EG no;
    	FR no;
    	FI no;
    }

  
This code sets the variable `$allow_visit` to yes or no, depending on the
country of the visitor. In this example, we have set `$allow_visit` to no for
Egypt, France and Finland, and yes for all other countries. Alternately, if
you want to block all countries except for a few, set the default to no and
set the variable to yes for a select few. A list of country codes can be found
[here][2].

Now we need to use the `$allow_visit` variable to block visitors. Inside your
server block, add this :

    
    if ($allow_visit = no) {
    	return 403;
    }

  
This will return the "403 Forbidden" page for all visitors from countries
whose country code is set to no. You can also define a custom 403 page which
is a bit more helpful than the default one.

    
    error_page 403 /custom_403.html;

  
It is also possible to use this for redirecting users based on their country,
but that'll be explored in another post.

   [1]: http://wiki.nginx.org/NginxHttpGeoIPModule
   [2]: http://www.maxmind.com/app/iso3166

