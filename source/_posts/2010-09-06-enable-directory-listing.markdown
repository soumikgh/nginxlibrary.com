---
author: Soumik Ghosh
date: '2010-09-06 06:43:53'
layout: post
comments: true
slug: enable-directory-listing
status: publish
title: Enable directory listing
wordpress_id: '7'
categories:
- Server Configuration
tags:
- directory listing
---

Enabling directory listing in a folder in nginx is
simple enough with just an `autoindex on;` directive inside the location
directive. You can also enable sitewide directory listing by putting it in the
server block or even enable directory access for all sites by putting it in the http block.

An example config file:

    
    server {
            listen   80;
            server_name  domain.com www.domain.com;
            access_log  /var/...........................;
            root   /path/to/root;
            location / {
                    index  index.php index.html index.htm;
            }
            location /somedir {
                   autoindex on;
            }
    }

A live example can be found [here][1].

   [1]: http://animorphsfanforum.com/fanart/2064/

