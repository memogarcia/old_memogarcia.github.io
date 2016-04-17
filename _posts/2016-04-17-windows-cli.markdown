---
layout: post
title:  "Windows command line interface"
date:   2016-04-17 19:55:11
categories: windows cli powershell cmd
---

Download a file

{% highlight powershell %}

  powershell -Command "(New-Object Net.WebClient).DownloadFile('url', 'destination')"

{% endhighlight %}


Windows event log

{% highlight powershell %}

  # use Event Viewer

{% endhighlight %}


Interact with services

{% highlight powershell %}

  services.msc

{% endhighlight %}
