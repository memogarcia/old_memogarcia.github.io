---
layout: post
title:  "Powershell notes"
date:   2015-08-11 10:51:00
categories: powershell windows
---

Tail command equivalent

{% highlight powershell %}

Get-Content file.log -Wait

{% endhighlight %}


touch equivalent 

{% highlight powershell %}

New-Item file.log -type file

{% endhighlight %}