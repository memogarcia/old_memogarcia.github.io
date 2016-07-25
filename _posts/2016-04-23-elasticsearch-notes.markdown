---
layout: post
title:  "Elasticsearch notes"
date:   2016-04-23 12:30:00
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
# or
sudo service elasticsearch start
{% endhighlight %}


Simple script to configure elasticsearch

{% highlight bash %}

#!/usr/bin/env bash

sudo apt-get install openjdk-7-jre -y

wget https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.7.5.deb

sudo dpkg -i elasticsearch-1.7.5.deb

sudo service elasticsearch start

{% endhighlight %}


Delete data from a specific type

{% highlight bash %}

curl -XDELETE 'http://localhost:9200/freezer/backups/'

{% endhighlight %}


Force an index to write data in all replicas

{% highlight bash %}

curl -XPOST 'http://localhost:9200/freezer/_refresh'

{% endhighlight %}

{% highlight python %}

# python
es.indices.refresh('freezer')

{% endhighlight %}
