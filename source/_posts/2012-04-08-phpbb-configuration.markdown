---
author: Soumik Ghosh
date: '2012-04-08 20:02:53'
layout: post
comments: true
slug: nginx-configuration-for-phpbb
status: publish
title: Nginx configuration for phpBB
categories:
- Application-specific configuration
tags:
- htaccess
- phpBB
---

I had previously [written an article](http://nginxlibrary.com/converting-phpbbs-default-htaccess-for-nginx/) about converting phpBB's default htaccess rules to Nginx. Here I'll share a full-blown Nginx configuration file for serving a phpBB forum.

This configuration file :

>-   Redirects www sub domain to the bare domain along with any parked domains.
>-   Prevents external access to files and directories like config.php, avatar upload directory, etc.
>-   Sets proper Expires header for images.

	
	server {
		listen   [::]:80;
		server_name  www.domain.com parkeddomain.com *.parkeddomain.com;
		rewrite ^    http://domain.com$request_uri? permanent;
	}
	
	server {
		listen   [::]:80;
		server_name  domain.com;
		root   /var/www/domain.com;
		index  index.php index.html index.htm;
		access_log  /var/logs/domain.com.access.log;
	
		location ~ /(config\.php|common\.php|cache|files|images/avatars/upload|includes|store) {
			deny all;
			return 403;
		}
	
		location ~* \.(gif|jpe?g|png|css)$ {
			expires   30d;
		}
	
		location ~ \.php$ {
			try_files $uri =404;
			fastcgi_pass   unix:/tmp/php.socket;
			fastcgi_index  index.php;
			fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
			include fastcgi_params;
		}
	}








