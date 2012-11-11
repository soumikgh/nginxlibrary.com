---
author: Soumik Ghosh
date: '2011-05-25 06:42:04'
layout: post
comments: true
slug: nginx-1-0-3-has-been-released
status: publish
title: Nginx 1.0.3 has been released
wordpress_id: '122'
categories:
- New Releases
tags:
- 1.0.3
---

Nginx 1.0.3 has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include :

  

  * Feature: the "auth_basic_user_file" directive supports "$apr1", "{PLAIN}", and "{SSHA}" password encryption methods. Thanks to Maxim Dounin.
  

  * Feature: the "geoip_org" directive and $geoip_org variable. Thanks to Alexander Uskov, Arnaud Granal, and Denis F. Latypoff.
  

  * Feature: ngx_http_geo_module and ngx_http_geoip_module support IPv4 addresses mapped to IPv6 addresses.
  

  * Bugfix: a segmentation fault occurred in a worker process during testing IPv4 address mapped to IPv6 address, if access or deny rules were defined only for IPv6; the bug had appeared in 0.8.22.
  

  * Bugfix: a cached reponse may be broken if proxy/fastcgi/scgi/ uwsgi_cache_bypass and proxy/fastcgi/scgi/uwsgi_no_cache directive values were different; the bug had appeared in 0.8.46.
  
   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?27,200796,200796

