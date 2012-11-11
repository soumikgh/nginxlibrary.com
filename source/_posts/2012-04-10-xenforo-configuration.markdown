---
author: Soumik Ghosh
date: '2012-04-10 19:45:53'
layout: post
comments: true
slug: nginx-configuration-for-xenforo
status: publish
title: Nginx configuration for XenForo
categories:
- Application-specific configuration
tags:
- XenForo
---

Writing a configuration file for XenForo is pretty simple and very similar to the [Wordpress configuration](http://nginxlibrary.com/wordpress-permalinks/) where requests are internally redirected to the index.php file. With this configuration XenForo's friendly URLs work perfectly, as well as external access is cut off for some directories like `libraries`
 and `internal_data`.

	server {
		listen   [::]:80;
		server_name  example.com www.example.com;
		root   /var/www/example.com;
		index  index.html index.htm index.php;
		access_log  /var/www/logs/example.com.access.log;  

		location / {
			try_files $uri $uri/ /index.php?$uri&$args;
		}

		location ~ /(internal_data|library) {
			 internal;
		}

		location ~ \.php$ {
			fastcgi_pass   unix:/tmp/php.socket;
			fastcgi_index  index.php;
			fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
			include fastcgi_params;
		}	
	}

For a XenForo installation in a sub directory `xenforo`, the configuration should be like this:

	server {
		listen   [::]:80;
		server_name  example.com www.example.com;
		root   /var/www/example.com;
		index  index.html index.htm index.php;
		access_log  /var/www/logs/example.com.access.log;  

		location /xenforo/ {
			try_files $uri $uri/ /xenforo/index.php?$uri&$args;
		}

		location ~ /xenforo/(internal_data|library) {
			 internal;
		}

		location ~ \.php$ {
			fastcgi_pass   unix:/tmp/php.socket;
			fastcgi_index  index.php;
			fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
			include fastcgi_params;
		}	
	}


