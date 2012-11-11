---
author: Soumik Ghosh
date: '2011-08-30 10:55:51'
layout: post
comments: true
slug: nginx-1-0-6-has-been-released
status: publish
title: Nginx 1.0.6 has been released
wordpress_id: '155'
categories:
- New Releases
tags:
- 1.0.6
---

Nginx 1.0.6 has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include :

  * Feature: cache loader run time decrease.

  * Feature: loading time decrease of configuration with large number of HTTPS sites.

  * Feature: now nginx supports ECDHE key exchange ciphers. Thanks to Adrian Kotelba.

  * Feature: the "lingering_close" directive.

  * Feature: now shared zones and caches use POSIX semaphores on Solaris. Thanks to Den Ivanov.

  * Bugfix: nginx could not be built on Linux 3.0.

  * Bugfix: a segmentation fault might occur in a worker process if "fastcgi/scgi/uwsgi_param" directives were used with values starting with "HTTP_"; the bug had appeared in 0.8.40.

  * Bugfix: in closing connection for pipelined requests.

  * Bugfix: nginx did not disable gzipping if client sent "gzip;q=0" in "Accept-Encoding" request header line.

  * Bugfix: in timeout in unbuffered proxied mode.

  * Bugfix: memory leaks when a "proxy_pass" directive contains variables and proxies to an HTTPS backend.

  * Bugfix: in parameter validaiton of a "proxy_pass" directive with variables. Thanks to Lanshun Zhou.

  * Bugfix: SSL did not work on QNX.

  * Bugfix: SSL modules could not be built by gcc 4.6 without --with-debug option.

   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?27,214440

