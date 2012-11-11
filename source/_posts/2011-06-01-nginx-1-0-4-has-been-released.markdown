---
author: Soumik Ghosh
date: '2011-06-01 06:40:52'
layout: post
comments: true
slug: nginx-1-0-4-has-been-released
status: publish
title: Nginx 1.0.4 has been released
wordpress_id: '121'
categories:
- New Releases
tags:
- 1.0.4
---

Nginx 1.0.4 has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include :

  * Change: now regular expressions case sensitivity in the "map" directive is given by prefixes "~" or "~*".
  * Feature: now shared zones and caches use POSIX semaphores on Linux. Thanks to Denis F. Latypoff.
  * Bugfix: "stalled" cache updating" alert.
  * Bugfix: nginx could not be built --without-http_auth_basic_module; the bug had appeared in 1.0.3.

   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?2,202918,207036

