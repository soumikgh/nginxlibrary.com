---
author: Soumik Ghosh
date: '2011-02-23 05:09:18'
layout: post
comments: true
slug: perl-fastcgi
status: publish
title: Setting up Perl FastCGI with Nginx
wordpress_id: '36'
categories:
- Server Configuration
tags:
- perl
- perl-fastcgi
---

This article assumes that you are running on Debian Lenny (or above) and uses
aptitude/apt-get for fetching and installing packages.

Install these packages :
    
    apt-get install nginx libfcgi-perl wget

Now we replace Nginx's package maintainer's vhosts file with our own.

    
    mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.old
    vi /etc/nginx/sites-available/default

Put these as the file contents :

```
server {
	listen   80;
	server_name  example.com www.example.com;
	root   /var/www/example.com;
	access_log  /var/www/logs/example.com.access.log;  

	location / {
		index  index.html index.htm index.pl;
	}  

	location ~ \.pl|cgi$ {
		try_files $uri =404;
		gzip off;
		fastcgi_pass  127.0.0.1:8999;
		fastcgi_index index.pl;
		fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
		include fastcgi_params;
  	} 
}
```

Now let us create the directory where the site will reside and assign proper
ownership to it.

    
    mkdir /var/www/example.com
    chown -R www-data:www-data /var/www/example.com

Next, we should download and configure a FastCGI wrapper script (credit: [Denis
S. Filimonov][1]), a Debian init script to start/stop the FastCGI process, give
them the necessary permissions to be executed and set the init script to start
up the FastCGI daemon automatically.

    
    wget http://nginxlibrary.com/downloads/perl-fcgi/fastcgi-wrapper -O /usr/bin/fastcgi-wrapper.pl
    wget http://nginxlibrary.com/downloads/perl-fcgi/perl-fcgi -O /etc/init.d/perl-fcgi
    chmod +x /usr/bin/fastcgi-wrapper.pl
    chmod +x /etc/init.d/perl-fcgi
    update-rc.d perl-fcgi defaults
    insserv perl-fcgi

Now that everything is done, let us start Nginx and test out a sample Perl
script.

    
    /etc/init.d/nginx start
    /etc/init.d/perl-fcgi start
    vi /var/www/example.com/index.pl

Put the following in the file :

``` perl
#!/usr/bin/perl

print "Content-type:text/html\n\n";
print <<EndOfHTML;
<html><head><title>Perl Environment Variables</title></head>
<body>
<h1>Perl Environment Variables</h1>
EndOfHTML

foreach $key (sort(keys %ENV)) {
	print "$key = $ENV{$key}<br>\n";
}

print "</body></html>";
```

Let us make the script executable by issuing the following command :
   
	chmod a+x /var/www/example.com/index.pl

Now, go to example.com and you'll see a list of Perl environment variables if
everything was done correctly.

   [1]: http://www.ruby-forum.com/topic/145858#645832

