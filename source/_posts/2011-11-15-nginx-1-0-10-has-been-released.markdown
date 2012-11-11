---
author: Soumik Ghosh
date: '2011-11-15 07:11:03'
layout: post
comments: true
slug: nginx-1-0-10-has-been-released
status: publish
title: Nginx 1.0.10 has been released
wordpress_id: '169'
categories:
- New Releases
tags:
- 1.0.10
---

Nginx 1.0.10 has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include :

  * Bugfix: a segmentation fault might occur in a worker process if resolver got a big DNS response. Thanks to Ben Hawkes.

  * Bugfix: in cache key calculation if internal MD5 implementation was used; the bug had appeared in 1.0.4.

  * Bugfix: the module ngx_http_mp4_module sent incorrect "Content-Length" response header line if the "start" argument was used. Thanks to Piotr Sikora.

   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?27,218365

