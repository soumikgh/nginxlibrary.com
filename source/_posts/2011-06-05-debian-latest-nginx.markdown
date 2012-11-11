---
author: Soumik Ghosh
date: '2011-06-05 04:32:39'
layout: post
comments: true
slug: debian-latest-nginx
status: publish
title: Installing the latest version of Nginx on Debian
wordpress_id: '48'
categories:
- Server Configuration
tags:
- apt-pinning
- debian
---

The Nginx package in the Debian stable repositories can be quite outdated. It
is often several version behind the latest, whereas the Nginx package from the
testing/unstable repositories is updated frequently with newer versions of
Nginx. We can use apt-pinning to get the a newer version of Nginx (the one
present in the testing/unstable repositories) and it's dependencies (if
required) will be met from the testing/unstable repositories as well.

Let us first add the testing/unstable repositories to the software sources
list.
    
    vi /etc/apt/sources.list

Add these lines at the end of the file :
    
    deb http://ftp.debian.org/debian/ testing main contrib non-free
    deb-src http://ftp.debian.org/debian/ testing main contrib non-free

Replace `testing` with `unstable` if you want to add the unstable repositories
instead of testing.

Now we need to pin the nginx package in testing/unstable to a higher priority
so that it gets selected. Actually, in Debian, nginx is a meta-package that
selects the [nginx-full][1] package. So, if you want to use [nginx-light][2]
or [nginx-extras][3], use them instead of just "nginx".
    
    vi /etc/apt/preferences

Add these lines :
    
    Package: nginx
    Pin: release a=testing
    Pin-Priority: 900

As explained before, you can use nginx-light or nginx-extra instead of nginx
here. You can also use unstable instead of testing.

Now, run :
    
    apt-get update

Check whether the newer version of nginx is selected for install by running :
    
    apt-cache policy nginx

Finally, install/upgrade nginx by running :

    
    apt-get install nginx

or

    
    apt-get upgrade nginx

  
You'll have a relatively newer version of Nginx installed after finishing
this.

   [1]: http://packages.debian.org/testing/nginx-full
   [2]: http://packages.debian.org/testing/nginx-light
   [3]: http://packages.debian.org/testing/nginx-extras

