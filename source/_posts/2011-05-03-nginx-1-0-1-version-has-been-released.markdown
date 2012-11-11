---
author: Soumik Ghosh
date: '2011-05-03 13:16:44'
layout: post
comments: true
slug: nginx-1-0-1-version-has-been-released
status: publish
title: Nginx 1.0.1 version has been released
wordpress_id: '75'
categories:
- New Releases
tags:
- 1.0.1
---

Nginx 1.0.1 version has been released.

Download it : [http://nginx.org/en/download.html][1]

[The changes][2] include:

  * Change: now the "split_clients" directive uses MurmurHash2 algorithm because of better distribution. Thanks to Oleg Mamontov.
  * Change: now long strings starting with zero are not considered as false values. Thanks to Maxim Dounin.
  * Change: now nginx uses a default listen backlog value 511 on Linux.
  * Feature: the $upstream_... variables may be used in the SSI and perl modules.
  * Bugfix: now nginx limits better disk cache size. Thanks to Oleg Mamontov.
  * Bugfix: a segmentation fault might occur while parsing incorrect IPv4 address; the bug had appeared in 0.9.3. Thanks to Maxim Dounin.
  * Bugfix: nginx could not be built by gcc 4.6 without --with-debug option.
  * Bugfix: nginx could not be built on Solaris 9 and earlier; the bug had appeared in 0.9.3. Thanks to Dagobert Michelsen.
  * Bugfix: $request_time variable had invalid values if subrequests were used; the bug had appeared in 0.8.47. Thanks to Igor A. Valcov.

   [1]: http://nginx.org/en/download.html (Download Nginx)
   [2]: http://forum.nginx.org/read.php?27,195215,195215

