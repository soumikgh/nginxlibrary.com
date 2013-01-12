---
author: Soumik Ghosh
date: '2012-01-09 15:29:45'
layout: post
comments: true
slug: 403-forbidden
status: publish
title: Resolving "403 Forbidden" error
categories:
- General Configuration
tags:
- '403'
- '403 Forbidden'
---

403 Forbidden errors are Nginx's way of telling "You have requested for a resource but we cannot give it to you." 403 Forbidden is technically not an error but a HTTP status code. 403 response headers are intentionally returned in many cases such as -

* User is blocked from requesting that page/resource or the site as a whole.
* User tries to access a directory but `autoindex` is set to `off`.
* User tries to access a file that can be only accessed internally.

These are some among many cases where a 403 Forbidden response is intentionally returned. But here we will talk about the causes of 403 responses that are unintentional/not desired which generally occur as a result of misconfiguration on the server side.

###Permissions are not set correctly

This is the most common cause of this error. By permissions, I do not only mean the permissions for the file that is being accessed. In order to serve a file, Nginx needs to have _read_ permissions for the file as well as _execute_ permissions for every hierarchial parent directory of the file to chdir to it. For example, to access the file located at -

	/usr/share/myfiles/image.jpg

Nginx needs to have _read_ permissions for the file as well as _execute_ permissions for `/`, `/usr`, `/usr/share` and `/usr/share/myfiles`. If you use the standard 755 for directories and 644 for files (umask: 022), you should not run into this problem. To check for ownership and permissions on a path, we can use the `namei` utility like this -

	namei -l /path/to/check

###Directory index is not properly defined

Sometimes, the index directive does not contain the desired directory index. For example, for a standard setup with PHP, the index directive should be -

	index index.html index.htm index.php;

According to this example, when a directory is acessed directly, Nginx will try to serve index.html, then index.htm and index.php after that. If none of them are found, Nginx will return a 403 header. If index.php were not defined in the root directive, Nginx would have returned 403 without checking for the existence of index.php.

Similarly, for a Python setup, index.py should be defined as a directory index.

These are the most common causes of undesired 403 responses. Feel free to leave a comment if you are still getting 403s.


