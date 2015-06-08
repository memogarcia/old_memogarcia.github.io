---
layout: post
title:  "Install elasticsearch"
date:   2015-06-08 10:45:00
categories: elasticsearch
---

Install OpenJDK (or java)

{% highlight bash %}
sudo apt-get install openjdk-7-jre
{% endhighlight %}

Download elasticsearch from:

{% highlight bash %}
https://www.elastic.co/downloads/elasticsearch
{% endhighlight %}

Install .deb file

{% highlight bash %}
sudo dpkg -i elasticsearch.deb
{% endhighlight %}

Start elasticsearch

{% highlight bash %}
sudo /etc/init.d/elasticsearch start
{% endhighlight %}