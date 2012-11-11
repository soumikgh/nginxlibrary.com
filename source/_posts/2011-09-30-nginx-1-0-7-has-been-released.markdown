---
author: Soumik Ghosh
date: '2011-09-30 14:59:02'
layout: post
comments: true
slug: nginx-1-0-7-has-been-released
status: publish
title: Nginx 1.0.7 has been released
wordpress_id: '160'
categories:
- New Releases
tags:
- 1.0.7
---

Nginx 1.0.7 has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include :

  * Change: now if total size of all ranges is greater than source response size, then nginx disables ranges and returns just the source response.

  * Feature: the "max_ranges" directive.

  * Feature: the module ngx_http_mp4_module.

  * Feature: the "worker_aio_requests" directive.

  * Bugfix: if nginx was built --with-file-aio it could not be run on Linux kernel which did not support AIO.

  * Bugfix: in Linux AIO error processing. Thanks to Hagai Avrahami.

  * Bugfix: in Linux AIO combined with open_file_cache.

  * Bugfix: open_file_cache did not update file info on retest if file was not atomically changed.

  * Bugfix: reduced memory consumption for long-lived requests.

  * Bugfix: in the "proxy/fastcgi/scgi/uwsgi_ignore_client_abort" directives.

  * Bugfix: nginx could not be built on MacOSX 10.7.

  * Bugfix: in the "proxy/fastcgi/scgi/uwsgi_ignore_client_abort" directives.

  * Bugfix: request body might be processed incorrectly if client used pipelining.

  * Bugfix: in the "request_body_in_single_buf" directive.

  * Bugfix: in "proxy_set_body" and "proxy_pass_request_body" directives if SSL connection to backend was used.

  * Bugfix: nginx hogged CPU if all servers in an upstream were marked as "down".

  * Bugfix: a segmentation fault might occur during reconfiguration if ssl_session_cache was defined but not used in previous configuration.

  * Bugfix: a segmentation fault might occur in a worker process if many backup servers were used in an upstream.

   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?27,216114

