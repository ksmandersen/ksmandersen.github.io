---
layout: post
title: "Static site development with pow"
date: 2013-01-26 12:13
comments: true
---

Ever had to set up Apache and VirtualHosts for local web developemnt?
Ever had to do lots of static site development and then test it on lots of devices? Then you'll love [Pow](http://pow.cx).

Pow is a small webserver created by the briliant guys at [37signals](http://37signals.com). It was developed for making local devlopment of
Ruby on Rails apps easier but it works just as well for static website.

Getting Pow up and running is very easy. Simply fire up your terminal and run the installer like this:

{% highlight bash %}
curl get.pow.cx | sh
{% endhighlight %}

Then add your local website as a host like so:


{% highlight bash %}
cd ~/.pow
ln -s /path/to/myapp
{% endhighlight %}

Now getting to your website is as easy as pointing your browser at ``http://myapp.dev``. Using pow with static sites
does have one minor caveat. Because Pow is developed for Rails apps you have to put all your static files inside a
directory named ``public`` in your website directory. There probably is a way around this but I haven't found it yet.

Pow makes it very conevenient to test your websites on other devices too. With pow and [xip.io](http://xip.io) you
can reach your websites from any device within your local network. Say your local website is at ``myapp.dev`` and your
local ip address is ``192.168.1.100`` then you pick your iPad and go to ``http://myapp.192.178.1.100.xip.io`` and test your
website.