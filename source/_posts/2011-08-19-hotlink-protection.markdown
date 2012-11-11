---
author: Soumik Ghosh
date: '2011-08-19 02:30:57'
layout: post
comments: true
slug: hotlink-protection
status: publish
title: Hotlink protection in Nginx
wordpress_id: '93'
categories:
- General Configuration
tags:
- hotlink protection
---

Hotlinked files can be a major cause for bandwidth leeching for some sites.
Here's how you can hotlink protect your images and other file types using a
simple location directive in your Nginx configuration file :

    location ~ \.(jpe?g|png|gif)$ {
         valid_referers none blocked mysite.com *.mysite.com;
         if ($invalid_referer) {
            return   403;
        }
    }

Use the pipe ("|") to separate file extensions you want to hotlink protect.

The `valid_referers` directive contains the list of site for whom hotlinking
is allowed. Here is an explanation of the parameters for the `valid_referers`
directive :

*  none - Matches the requests with no Referrer header.
*  blocked - Matches the requests with blocked Referrer header.
*  *.mydomain.com - Matches all the sub domains of mydomain.com. Since v0.5.33, * wildcards can be used in the server names.

You can also tweak the location directive for blocking files from a specific
directory. Like this :

    
    location /images/ {
         valid_referers none blocked mysite.com *.mysite.com;
         if ($invalid_referer) {
            return   403;
        }
    }

This will hotlink protect every file in any images directory.

