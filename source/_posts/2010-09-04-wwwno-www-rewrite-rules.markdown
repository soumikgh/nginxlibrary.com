---
author: Soumik Ghosh
date: '2010-09-04 09:44:37'
layout: post
comments: true
slug: wwwno-www-rewrite-rules
status: publish
title: Www/no-www rewrite rules
wordpress_id: '525'
categories:
- General Configuration
tags:
- nginx
- non-www
- rewrite
---

There are many ways to rewrite www urls to their non-www
versions in nginx. Here one that's Igor-approved and works well on my setup :

WWW to Non-WWW:

	#301 redirect www to non-www
	server {
		listen   [::]:80;
		server_name  www.domain.com;
		rewrite ^    http://domain.com$request_uri? permanent;
	}
	
	server {
		listen   [::]:80;
		server_name  domain.com;
		.........................................
		.........................................
	}

Non-WWW to WWW:

	#301 redirect non-www to www
	server {
		listen   [::]:80;
		server_name  domain.com;
		rewrite ^    http://www.domain.com$request_uri? permanent;
	}

	server {
		listen   [::]:80;
		server_name  www.domain.com;
		.........................................
		.........................................
	}


