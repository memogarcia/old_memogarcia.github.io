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


Using thread pools https://docs.python.org/dev/library/multiprocessing.html#multiprocessing.pool.Pool

{% highlight python %}

import urllib2
from multiprocessing.dummy import Pool as thread_pool


urls = ["python.org",]

# number of threads
pool = thread_pool(8)

results = pool.map(urllib2.open, urls)

pool.close()
pool.join()

{% endhighlight %}