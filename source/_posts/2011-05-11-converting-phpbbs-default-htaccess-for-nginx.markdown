---
author: Soumik Ghosh
date: '2011-05-11 11:50:53'
layout: post
comments: true
slug: converting-phpbbs-default-htaccess-for-nginx
status: publish
title: Converting phpBB's default .htaccess for nginx
wordpress_id: '22'
categories:
- General Configuration
- Application-specific configuration
tags:
- htaccess
- phpbb
---

phpBB ships with a small .htaccess file in the root that has rules to
deny access to the config.php and common.php files in the absence of a PHP
interpreter. I'll duplicate those rules for an Nginx configuration.

Here's how the .htaccess file looks like :

	Order Allow,Deny
	Deny from All

	Order Allow,Deny
	Deny from All

And here's what you need to add to your Nginx configuration file in order to
duplicate that behaviour :

	location ~ /config.php|/common.php {
		deny all;
	}

This will deny access to all instances of config/common.php located in the
document root, regardless of where they are located.

