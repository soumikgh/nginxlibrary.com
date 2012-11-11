---
author: Soumik Ghosh
date: '2011-07-30 01:58:11'
layout: post
comments: true
slug: hide-nginx-version
status: publish
title: Hide Nginx version
wordpress_id: '140'
categories:
- General Configuration
tags:
- http header
- version
---

If you are using an old version of Nginx or if you want to explicitly hide the Nginx version in your HTTP headers and your default error pages, you can use the `server_tokens` directive in your Nginx configuration. The server_tokens directive can be used either in the http, server or location block.

Just set `server_tokens` to `off` (it is set to `on` by default) to hide your Nginx version in all externally visible places.

	server_tokens off;

Just like using Apache's ServerSignature and ServerTokens directive, your Nginx version should now be hidden in the server signature and the default error pages. To check the server headers, use the [HTTP Header Checker utility][1] from Webconfs.com.

   [1]: http://www.webconfs.com/http-header-check.php (HTTP Header Checker)

