---
layout: post
title:  "Ifconfig tool"
date:   2016-06-05 12:30:00
categories: ifconfig network linux
---

How to get bus drive information from network card

{% highlight bash %}

sudo apt install ethtool
ethtool -i eth0

{% endhighlight %}


How to enable/disable an interface

{% highlight bash %}

sudo ifconfig eth0 up
sudo ifconfig eth0 down

{% endhighlight %}


How to assign an ip address to an interface

{% highlight bash %}

sudo ifconfig eth0 192.168.33.10

{% endhighlight %}


How to assign a broadcast ip address to an interface

{% highlight bash %}

sudo ifconfig eth0 broadcast 192.168.33.255

{% endhighlight %}


How to enable/disable promiscuous mode

{% highlight bash %}

sudo ifconfig eth0 promisc
sudo ifconfig eth0 -promisc

{% endhighlight %}


How to change mac address of the interface

{% highlight bash %}

sudo ifconfig eth0 hw ether AA:BB:CC:DD:EE:FF

{% endhighlight %}
