---
author: Soumik Ghosh
date: '2010-09-03 10:29:45'
layout: post
comments: true
slug: resolving-no-input-file-specified-error
status: publish
title: Resolving "No input file specified" error
wordpress_id: '511'
categories:
- General Configuration
tags:
- '404'
- nginx
- php-cgi
---

If you are using nginx with php-cgi and have followed the standard procedure to set it up, you might often get the "No input file specified" error. This error basically occurs when the php-cgi daemon cannot find a `.php` file to execute using the `SCRIPT_FILENAME` parameter that was supplied to it. I'll discuss about the common causes of the error and it's solutions.

###Wrong path is sent to the php-cgi daemon

More often than not, a wrong path (`SCRIPT_FILENAME`) is sent to the fastCGI daemon. In many of the cases, this is due to a misconfiguration. Some of the setups I have seen are configured like this :

	server {
		listen   [::]:80;
		server_name  example.com www.example.com;
		access_log  /var/www/logs/example.com.access.log;  

		location / {
			root   /var/www/example.com;
			index  index.html index.htm index.pl;
		}
		
		location /images {
			autoindex on;
		}

		location ~ \.php$ {
			fastcgi_pass   127.0.0.1:9000;
			fastcgi_index  index.php;
			fastcgi_param  SCRIPT_FILENAME  /var/www/example.com$fastcgi_script_name;
			include fastcgi_params;
		}
	}

Now, there are many things wrong with this configuration. An obvious and glaring issue is the `root` directive in the `location /` block. When the root is defined inside the location block, it is available/defined for that block only. Here, the `location /images` block will not match for any request because it does not have any `$document _root` defined and we will have to redundantly define root again for it. Obviously, the root directive should be moved out of the `location /` block and defined in the `server` block. This way, the location blocks will inherit the value defined in the parental server block. Of cource, if you want to define a different `$document_root` for a location, you can put a root directive in a location block.

Another issue is that the value of the fastCGI parameter `SCRIPT_FILENAME` is hard-coded. If we change the value of the root directive and move our files somewhere else in the directory chain, php-cgi will return a "No input file specified" error because will not be able to find the file in the hard-coded location which didn't change when the `$document_root` was changed. So, we should set SCRIPT_FILENAME as below :

	fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;

We should keep in mind that the root directive should be in the server block or else, only the `$fastcgi_script_name` will get passed as the SCRIPT_FILENAME and we will get the "No input file specified" error.

###The file actually does not exist

What happens here is that, even when Nginx receives the request to serve a **non-existent** file with a `.php` extension, it passes the request to php-cgi. This happens because Nginx does not check for the file, it only checks if the `REQUEST_URI` ends with a `.php`. Php-cgi, while trying to processs the request, finds that the php file does not exist at all. Hence it sends a "No input file specified" message with a "404 Not Found" header.

We can intercept the request for the non-existent php file and show a 404 page.

First, find out the version of nginx you are using.

	nginx -v

You'll get an output like this :

	nginx version: nginx/0.7.67

If php-cgi is running on port 9000, you'll have something like this in your
vhosts file :

	location ~ .php$ {
		fastcgi_pass   127.0.0.1:9000;
		fastcgi_index  index.php;
		fastcgi_param  SCRIPT_FILENAME ....
		...................................
		...................................
	}

Since nginx versions less than 0.6.36 doesn't have the try_files directive,
we'll have two versions of the code.

**For nginx 0.6.36+**

We'll use try_files here to catch non-existent URLs and display an error page. 
 
	location ~ .php$ {
		try_files  $uri =404;
		fastcgi_pass   127.0.0.1:9000;
		fastcgi_index  index.php;
		fastcgi_param  SCRIPT_FILENAME ....
		...................................
		...................................
	}

Here we are checking for the existence of the `.php` file before passing it to the fastCGI backend. If the file does not exist, we return a 404 Not Found page.

**For older versions :**  

	location ~ .php$ {
		fastcgi_intercept_errors on;
		error_page  404  /path/to/404.htm;
		fastcgi_pass   127.0.0.1:9000;
		fastcgi_index  index.php;
		fastcgi_param  SCRIPT_FILENAME ....
		...................................
		...................................
	}

Here the `fastcgi_intercept_errors` asks the fastCGI backend to return error statuses to Nginx so that Nginx can respond with a custm 404 page. The error_page directive defines a custom 404 page. Note that this will work for both older and newer versions of Nginx, but personally I think the try_catch method is cleaner.

###Permissions are not set correctly

Permissions are not supposed to be much of a headache for executing PHP files. Apparently, we only need to make sure that the user the fastCGI backend is running as, has read permissions for the file. But it is often overlooked that a user also needs execute permissions for every parent directory of a file to chdir to that file. For example, say a file is located at -

	/home/steve/files/req.txt

The user needs to have read permissions for the file as well as execute permissions for `/`, `/home`, `/home/steve` and `/home/steve/files`.

I have covered most of the common issues that cause this error. If you have something else, feel free to leave a comment.
