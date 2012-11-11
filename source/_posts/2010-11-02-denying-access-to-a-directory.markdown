---
author: Soumik Ghosh
date: '2010-11-02 06:02:14'
layout: post
comments: true
slug: denying-access-to-a-directory
status: publish
title: Denying access to a directory
wordpress_id: '31'
categories:
- General Configuration
---

Similar to Apache's `Deny from all`, nginx has the `deny all` directive.

To deny access to a directory called 'dirdeny' and return a "403 Forbidden"
header, use this code within your configuration file:

    
    location /dirdeny {
            deny all;
            return 403;
    }

