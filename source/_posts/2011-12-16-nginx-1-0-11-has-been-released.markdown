---
author: Soumik Ghosh
date: '2011-12-16 03:08:30'
layout: post
comments: true
slug: nginx-1-0-11-has-been-released
status: publish
title: Nginx 1.0.11 has been released
wordpress_id: '172'
categories:
- New Releases
tags:
- 1.0.11
---

Nginx 1.0.11 has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include :

  * Change: now double quotes are encoded in an "echo" SSI-command output. Thanks to Zaur Abasmirzoev.

  * Feature: the "image_filter_sharpen" directive.

  * Bugfix: a segmentation fault might occur in a worker process if SNI was used; the bug had appeared in 1.0.9.

  * Bugfix: SIGWINCH signal did not work after first binary upgrade; the bug had appeared in 1.0.9.

  * Bugfix: the "If-Modified-Since", "If-Range", etc. client request header lines might be passed to backend while caching; or not passed without caching if caching was enabled in another part of the configuration.

  * Bugfix: in the "scgi_param" directive, if complex parameters were used.

  * Bugfix: "add_header" and "expires" directives did not work if a request was proxied and response status code was 206.

  * Bugfix: in the "expires @time" directive.

  * Bugfix: in the ngx_http_flv_module. Thanks to Piotr Sikora.

  * Bugfix: in the ngx_http_mp4_module.

  * Bugfix: nginx could not be built on FreeBSD 10.

  * Bugfix: nginx could not be built on AIX.

   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?27,218365

