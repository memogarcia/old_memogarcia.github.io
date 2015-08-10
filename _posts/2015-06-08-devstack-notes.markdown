---
layout: post
title:  "Devstack notes"
date:   2015-06-08 10:45:00
categories: devstack
---

[Clone devstack](http://docs.openstack.org/developer/devstack/)

{% highlight bash %}
git clone https://git.openstack.org/openstack-dev/devstack
{% endhighlight %}

Make sure your vm network configuration is on bridge


set the proxies and no_proxies

{% highlight bash %}
export no_proxy=localhost,127.0.0.1,<your_ip>
{% endhighlight %}

Change connection protocol from git to https in stackrc

{% highlight bash %}
GIT_BASE=${GIT_BASE:-https://git.openstack.org}
{% endhighlight %}

If your ubuntu version is not supported run this:

{% highlight bash %}
FORCE=yes ./stack.sh
{% endhighlight %}


Create local.conf in devstack folder

{% highlight bash %}

[[local|localrc]]
#FIXED_RANGE=192.168.20.0/24
#NETWORK_GATEWAY=192.168.20.1a
LOGDAYS=1
LOGFILE=$DEST/logs/stack.sh.log
SCREEN_LOGDIR=$DEST/logs/screen
ADMIN_PASSWORD=quiet
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
SERVICE_TOKEN=a682f596-76f3-11e3-b3b2-e716f9080d50

{% endhighlight %}

Enable swift

{% highlight bash %}

SWIFT_REPLICAS=1
SWIFT_DATA_DIR=$DEST/data/swift
SWIFT_HASH=66a3d6b56c1f479c8b4e70ab5c2000f5
enable_service s-proxy s-object s-container s-account

{% endhighlight %}