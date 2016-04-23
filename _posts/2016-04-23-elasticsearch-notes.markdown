---
layout: post
title:  "Elasticsearch notes"
date:   2016-04-23 12:30:00
categories: elasticsearch
---

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
