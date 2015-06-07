---
layout: post
title:  "Spawn threads in python"
date:   2015-06-07 19:55:11
categories: python threads snippets
---
Simple snippet to spawn a thread in python using a decorator

{% highlight python %}

import threading

def spawn(func):
    thread = threading.Thread(target=func)
    thread.start()
    return thread

@spawn
def task():
    pass

task.join()

{% endhighlight %}

