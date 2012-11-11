---
author: Soumik Ghosh
date: '2011-07-19 06:42:10'
layout: post
comments: true
slug: nginx-1-0-5-has-been-released
status: publish
title: Nginx 1.0.5 has been released
wordpress_id: '123'
categories:
- New Releases
tags:
- 1.0.5
---

Nginx 1.0.5 has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include :

  * Change: now default SSL ciphers are "HIGH:!aNULL:!MD5". Thanks to Rob Stradling.
  * Feature: the "referer_hash_max_size" and "referer_hash_bucket_size" directives. Thanks to Witold Filipczyk.
  * Feature: $uid_reset variable.
  * Bugfix: a segmentation fault might occur in a worker process, if a caching was used. Thanks to Lanshun Zhou.
  * Bugfix: worker processes may got caught in an endless loop during reconfiguration, if a caching was used; the bug had appeared in 0.8.48. Thanks to Maxim Dounin.
  * Bugfix: "stalled cache updating" alert. Thanks to Maxim Dounin.

   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?27,212624

