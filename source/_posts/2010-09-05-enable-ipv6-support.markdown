---
author: Soumik Ghosh
date: '2010-09-05 13:29:53'
layout: post
comments: true
slug: enable-ipv6-support
status: publish
title: Enable IPv6 support
wordpress_id: '548'
categories:
- General Configuration
---

To enable IPv6 support in nginx, we need to check whether
it has been compiled with `--with-ipv6` flag. To check, fire up the terminal
and type in this command :

	nginx -V

The results should be something like this :

	nginx version: nginx/0.7.65
	TLS SNI support enabled
	configure arguments: --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --pid-path=/var/run/nginx.pid --lock-path=/var/lock/nginx.lock --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/lib/nginx/body --http-proxy-temp-path=/var/lib/nginx/proxy --http-fastcgi-temp-path=/var/lib/nginx/fastcgi--with-debug --with-http_stub_status_module --with-http_flv_module --with-http_ssl_module --with-http_dav_module --with-http_gzip_static_module --with-http_realip_module --with-mail --with-mail_ssl_module --with-ipv6 --add-module=/build/buildd/nginx-0.7.65/modules/nginx-upstream-fair

Pre-compiled Debian/Ubuntu packages already has IPv6 support built-in.

Now we need to edit the configuration file to tell nginx to bind to IPv6
addresses(as well as IPv4 addresses).

	sudo nano /etc/nginx/sites-available/default

Depending on your configuration, this file might be located somewhere else.
(Check `/usr/local/nginx/conf/nginx.conf` if you compiled nginx manually)

Search for listen directives and change them as follows:

	listen [::]:80;

This will make nginx bind to both IPv6 and IPv4 addresses.

In order to bind to IPv6 addresses only (no IPv4), use the following:

	listen [::]:80 default ipv6only=on;

In order to bind to a specific IPv6 address:

	listen [2001:470:0:76::2]:80;

When you are done, reload the nginx configuration file by typing in :

	nginx -s reload

Now we have to check whether nginx is listening to both IPv6 and IPv4
requests:

	netstat -tulpna | grep nginx

You'll get something like this :

	tcp6 0 0 :::80 :::* LISTEN 1891/nginx

You have successfully configured nginx to respond to IPv6 requests.

