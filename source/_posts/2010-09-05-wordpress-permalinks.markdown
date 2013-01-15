---
author: Soumik Ghosh
date: '2010-09-05 14:23:59'
layout: post
comments: true
slug: wordpress-permalinks
status: publish
title: Wordpress permalinks
wordpress_id: '562'
categories:
- General Configuration
- Application-specific configuration
tags:
- permalinks
- wordpress
---

Wordpress generally works out-of-the box on nginx. The
posts load fine, the functions in the dashboard work pretty well, until you
come to the permalinks. If you are on Apache, with mod_rewrite, Wordpress will
automatically add the required rewrite rules to your `.htaccess` file for
permalinks to work. But for nginx, you have to add the rules manually.

Moreover, when WordPress detects that mod_rewrite is not loaded (which is the
case with nginx), it falls back to using [`PATHINFO`][1] permalinks, which
inserts an extra 'index.php' in front. This hasn't been much of a problem for
me as I have been using the custom structure option to remove the index.php.
It has been working fine for me. (Screenshot below)

![2]

Apart from that, you will also need to edit your nginx configuration file to
make the permalinks work. We will use the [`try_files` directive][3]
(available from nginx 0.7.27+) to pass URLs to Wordpress's index.php for them
to be internally handled. This will also work for 404 requests.

If your blog is at the root of the domain (something like `http://www.myblog.com`), find the `location /` block inside the configuration file, and add the following line to it.

	try_files $uri $uri/ /index.php?$args;

Here, Nginx checks for the existence of a file at the URL (`$uri`), then for a directory (`$uri/`). If it doesn't find a directory or a file, it performs an internal redirect to `/index.php` passing the URL as pathinfo.
  
It should look like this after the edits :

	location / {
		index index.php index.html index.htm;
		try_files $uri $uri/ /index.php?$args;
	}

If your blog is in a subfolder (say /blog), you'll have to add an extra `location /blog/` block to your configuration file :

	location /blog/ {
		try_files $uri $uri/ /blog/index.php?q=$uri&$args;
	}

After you have finished making the changes in the configuration file, reload
the nginx configuration by :

	nginx -s reload

Wordpress' pretty permalinks should work fine now.


   [1]: http://codex.wordpress.org/Using_Permalinks#PATHINFO:_.22Almost_Pretty.22
   [2]: http://nginxlibrary.com/images/post-images/2010/nginx_wordpress_permalinks.jpg (nginx wordpress permalinks)
   [3]: http://wiki.nginx.org/NginxHttpCoreModule#try_files

