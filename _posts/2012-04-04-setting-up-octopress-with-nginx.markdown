---
layout: post
title: "Setting up Octopress with nginx"
date: 2012-04-04 14:50
published: true
---
I decided to write up this quick little post containing my basic nginx configuration. I did
it mostly in documentation for myself. However, should you find it helpful or interesting then
that's great.

The configuration featured in this post is general enough that it can be used for pretty
much any static website content.

I got most of my configuration from [this blogpost](http://blog.bigdinosaur.org/octopress-and-nginx/)
by [Lee (@bigdinosaur)](http://blog.bigdinosaur.org/). Great thanks to him. Without his post I probably
wouldn't have figured most of this stuff out, so quick.

Here's the general ```nginx.conf``` (which is most commonly located somewhere like ```/etc/nginx/conf/nginx.conf```):

{% highlight nginx %}
worker_processes 2;

events {
	worker_connections 1024;
}

http {
  include           mime-types;
  default_type      application/octet-stream;
	
  sendfile          on;
  keepalive_timeout 65;

  ### Enable gzip and deflate compression of files to save bandwidth.
  gzip              on;
  gzip_vary         on;
  gzip_comp_level   6;
  gzip_disable      "MSIE [1-6]\.(?!.*SV1)"; # Those pesky IE6 users!
  gzip_types        text/plain text/css application/json application/x-javascript
                    text/xml application/xml application/rss+xml text/javascript
                    application/javascript text/x-js image/svg+xml application/vnd.ms-fontobject
                    application/x-font-ttf font/opentype;

  include           /etc/nginx/sites-enabled/*;

}
{% endhighlight %}

One thing to notice here is that I'm running 2 worker processes for my server. Depending on your traffic
and how many CPU's or cores your server has you may wan't to adjust this.
[This post on lifeLinux](http://www.lifelinux.com/how-to-optimize-nginx-for-maximum-performance/) explains
this and other ways to optimize your nginx configuration.

Here's my site specific file (which should be somewhere like ```/etc/nginx/sites-available/yoursite.tld```):

{% highlight nginx %}
server {
  listen       80;
  server_name  yoursite.tld www.yourstite.tld;
  rewrite      ^ https://yoursite.tld$request_uri permanent;
}

server {
  ### Listen on port 443 for ssl connections and handle them as https requests
  listen 443 ssl;

  ### This setting tells Nginx to use this configuration if it gets a request for
  ### yoursite.tld
  server_name yoursite.tld;

  ### The next set of directives tells Nginx that this server uses SSL, and then
  ### supplies it with the configuration settings necessary to make SSL work
  ssl                       on;
  ssl_certificate           /path/to/certificate.crt;
  ssl_certificate_key       /path/to/private.key;
  ssl_session_timeout       5m;
  ssl_protocols             SSLv3 TLSv1;
  ssl_ciphers               ALL:!aNULL:!ADH:!eNULL:!LOW:!EXP:RC4+RSA:+HIGH:+MEDIUM;
  ssl_prefer_server_ciphers on;    

  ### This tells nginx to use "index.html" as the default index page everywhere
  index                     index.html;
	
  ### This is the location on the web server where your Octopress files are
  ### published. Setting this here means you don't have to set it for any of the
  ### individual locations you define below.
  root                      /path/to/public;

  ### This disables automatic directory index creation, since no one will be
  ### browsing your directories anyway
  autoindex                 off;

  ### Include the most common directives.
  include                   common.conf;

  ### Set up some basic caching rules to lower your traffic. Most common files
  ### like images, sound and movies are cached for 30 days.
  location ~* \.(ico|gif|jpe?g|png|flv|pdf|swf|mov|mp3|wmv|ppt)$ {
    access_log              off;
    expires                 30d;
    add_header              Cache-Control  "public";
  }

  ### Static content files like HTML, CSS and Javascript is cached for 3 days
  ### but must be revalidated.
  location ~* \.(xml|txt|html|htm|js|css)$ {
    access_log              off;
    expires                 3d;
    add_header              Cache-Control  "private, must-revalidate";
  }
}
{% endhighlight %}

It's worth noting here that I redirect my site (including ```www```) to ```https```.
If you only wan't to serve http you can just change the ```listen 443 ssl``` to ```listen 80```
and delete all the directivies prefixed with ssl.

At last is the common file. This file must be located in the same directory as the
```nginx.conf``` file:
{% highlight nginx %}
### Common root location
location / {
  try_files $uri $uri/ =404;
}

### Common deny, drop, or internal locations
location ~ /\. { access_log off; log_not_found off; deny all; }
location ~ ~$ { access_log off; log_not_found off; deny all; }
location = /robots.txt { access_log off; log_not_found off; } 
location = /favicon.ico { access_log off; log_not_found off; }
{% endhighlight %}

That's pretty much it. Have any questions or suggestions be sure to send be a [tweet](https://twitter.com/ksmandersen).