---
author: Soumik Ghosh
date: '2011-07-24 07:14:21'
layout: post
comments: true
slug: check-nginx-version
status: publish
title: Check Nginx version
wordpress_id: '134'
categories:
- General
tags:
- nginx version
---

We can retrieve the version of Nginx currently installed by calling the Nginx
binary with some command-line parameters. We can use the **-v** parameter to
display the Nginx version only, or use the **-V** parameter to display the
version, along with the compiler version and configuration parameters.

Example outputs :
    
    $ nginx -v
    nginx version: nginx/0.8.54
    
    $ nginx -V
    nginx version: nginx/0.8.54
    TLS SNI support enabled
    configure arguments: --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-log-path=/var/log/nginx/access.log --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --lock-path=/var/lock/nginx.lock --pid-path=/var/run/nginx.pid --with-debug --with-http_addition_module --with-http_dav_module --with-http_geoip_module --with-http_gzip_static_module --with-http_image_filter_module --with-http_realip_module --with-http_stub_status_module --with-http_ssl_module --with-http_sub_module --with-http_xslt_module --with-ipv6 --with-sha1=/usr/include/openssl --with-md5=/usr/include/openssl --with-mail --with-mail_ssl_module --add-module=/build/buildd/nginx-0.8.54/debian/modules/nginx-upstream-fair

