---
author: Soumik Ghosh
date: '2011-04-11 14:57:49'
layout: post
comments: true
slug: ubuntu-latest-nginx
status: publish
title: Installing the latest version of Nginx on Ubuntu
wordpress_id: '46'
categories:
- Server Configuration
tags:
- latest
- ubuntu
---

The Nginx release on the Ubuntu repositories is often outdated. Igor pushes
out new versions of Nginx quite frequently and it's understandable that the
Ubuntu package maintainers do not keep up. Also, Ubuntu only includes major
version updates with new OS releases. So, if the Nginx development branch
turns stable, and you plan on staying on your LTS (Long Term Support) version,
you might not be able to get the newer version of Nginx unless you upgrade
your OS.

So, in view of all this, [Michael Lustfield][1] and a bunch of other cool
folks maintain a Launchpad PPA [where they provide][2] the latest versions of
Nginx from both the stable and development branch. To get the latest version
of Nginx from the PPA, follow these steps :

	add-apt-repository ppa:nginx/stable
	apt-get update && apt-get install nginx

Use `ppa:nginx/development` to get the latest development version.

If you want to do it manually, you'll need to edit your sources.list, import
the appropriate key, and then install/upgrade Nginx.

	echo "deb http://ppa.launchpad.net/nginx/stable/ubuntu $(lsb_release -cs) main" >> /etc/apt/sources.list
	apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C300EE8C
	apt-get update && apt-get install nginx

Here too you can use `deb http://ppa.launchpad.net/nginx/development/ubuntu
$(lsb_release -cs) main` to get the latest development version.

You now have the latest version of Nginx on your server. Ubuntu will also
automatically pull down and install updates for Nginx if they are available.

   [1]: http://profarius.com/
   [2]: https://launchpad.net/~nginx

