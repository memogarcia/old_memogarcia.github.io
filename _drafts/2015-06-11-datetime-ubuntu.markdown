---
layout: post
title:  "Sync date and time in ubuntu"
date:   2015-06-11 14:45:00
categories: ubuntu linux datetime
---

This is very common when you pause a vm

{% highlight bash %}
sudo ntpdate ntp.ubuntu.com
{% endhighlight %}

Or if you need to change the date

{% highlight bash %}
sudo date --set "11 Jun 2015 14:49:00"
{% endhighlight %}