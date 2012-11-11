---
author: Soumik Ghosh
date: '2011-11-01 13:06:46'
layout: post
comments: true
slug: nginx-1-0-9-has-been-released
status: publish
title: Nginx 1.0.9 has been released
wordpress_id: '166'
categories:
- New Releases
tags:
- 1.0.9
---

Nginx 1.0.9 has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include :

  * Change: now the 0x7F-0x1F characters are escaped as \xXX in an access_log.

  * Change: now SIGWINCH signal works only in daemon mode.

  * Feature: "proxy/fastcgi/scgi/uwsgi_ignore_headers" directives support the following additional values: X-Accel-Limit-Rate, X-Accel-Buffering, X-Accel-Charset.

  * Feature: decrease of memory consumption if SSL is used.

  * Feature: accept filters are now supported on NetBSD.

  * Feature: the "uwsgi_buffering" and "scgi_buffering" directives. Thanks to Peter Smit.

  * Bugfix: a segmentation fault occurred on start or while reconfiguration if the "ssl" directive was used at http level and there was no "ssl_certificate" defined.

  * Bugfix: some UTF-8 characters were processed incorrectly. Thanks to Alexey Kuts.

  * Bugfix: the ngx_http_rewrite_module directives specified at "server" level were executed twice if no matching locations were defined.

  * Bugfix: a socket leak might occurred if "aio sendfile" was used.

  * Bugfix: connections with fast clients might be closed after send_timeout if file AIO was used.

  * Bugfix: in the ngx_http_autoindex_module.

  * Bugfix: the module ngx_http_mp4_module did not support seeking on 32-bit platforms.

  * Bugfix: non-cacheable responses might be cached if "proxy_cache_bypass" directive was used. Thanks to John Ferlito.

  * Bugfix: cached responses with an empty body were returned incorrectly; the bug had appeared in 0.8.31.

  * Bugfix: 201 responses of the ngx_http_dav_module were incorrect; the bug had appeared in 0.8.32.

  * Bugfix: in the "return" directive.

  * Bugfix: the "ssl_verify_client", "ssl_verify_depth", and "ssl_prefer_server_ciphers" directives might work incorrectly if SNI was used.

   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?27,217643

