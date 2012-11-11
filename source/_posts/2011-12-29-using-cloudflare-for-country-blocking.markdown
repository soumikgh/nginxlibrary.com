---
author: Soumik Ghosh
date: '2011-12-29 06:10:59'
layout: post
comments: true
slug: using-cloudflare-for-country-blocking
status: publish
title: Using CloudFlare for country blocking
wordpress_id: '175'
categories:
- General Configuration
---

{% img right http://nginxlibrary.com/images/post-images/2011/cloudflare-logo.png 244 40 %}

[CloudFlare][3] is an innovative service, providing
webmasters with a CDN (Content Delivery Network) and [many other things][4].
Among these other things, it provides an IP geolocation service using which we
can use to redirect/block visitors from specific countries.

We need to first enable 'IP Geolocation' in the CloudFlare settings for this
to work -

[![IP geolocation][5]][6]

We also need to make sure that the A record for the domain/sub domain is being
proxied though the CloudFlare proxy. So, enable CloudFlare for the relevant A
record from the DNS settings if it hasn't been set accordingly.

If everything is done properly, the HTTP_CF_IPCOUNTRY environment variable
should contain the country code for the visitor. To test it, we can write a
small script in Perl.

``` perl
#!/usr/bin/perl
print "Content-type: text/html\n\n$ENV{'HTTP_CF_IPCOUNTRY'}";
```

Run the script and it should show you your country code. A list of country
codes can be found [here][7].

Now, we need to take care of the Nginx configuration. We need to map the
environment variable to a variable of our own, so that the value of our
variable changes depending on whether we want to allow visitors from the
country or not.

We need to put the following code inside the http block but outside the server
block for your website. It can be put in `/etc/nginx/nginx.conf` inside the
HTTP block or in your vhosts file, outside of the server block.

    
    map $http_cf_ipcountry $allow {
            default yes;
            CN no;
            MX no;
            NO no;
    }

This sets `$allow` to yes or no, depending on the country of the visitor.
Here, we are setting the variable to no if the visitor is from China, Mexico,
or Norway. We can also set the default to no, and allow countries one by one
if we need to allow only a few countries and block all others. Now that the
variable is set, we need to instruct Nginx to serve a 403 page to the visitors
from the blocked countries. Add this anywhere inside the server block :
    
    if ($allow = no) {
    	return 403;
    }

This will make Nginx return a 403 Forbidden HTTP status code. A custom 403
error page can also be defined to make the error page more helpful.

If you are not using CloudFlare, you can still block visitors by country [using the GeoIP module][8].

   [3]: https://www.cloudflare.com/ (CloudFlare)
   [4]: https://www.cloudflare.com/overview (CloudFlare features overview)
   [5]: http://nginxlibrary.com/images/post-images/2011/ip_geolocation.jpg (IP geolocation)
   [6]: http://nginxlibrary.com/images/post-images/2011/ip_geolocation.jpg
   [7]: http://www.maxmind.com/app/iso3166
   [8]: http://nginxlibrary.com/ip-based-country-blocking/

