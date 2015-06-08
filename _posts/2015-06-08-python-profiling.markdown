---
layout: post
title:  "Python profiling"
date:   2015-06-08 15:23:00
categories: python profiling debug
---

Timer decorator

{% highlight python %}

import time
from functools import wraps


def timer(function):
    @wraps(function)
    def fn_timer(*args, **kwargs):
        start = time.time()
        result = function(*args, **kwargs)
        finish = time.time()
        print ("Total time running {0}: {1} seconds"
               .format(function.func_name, str(finish-start)))
        return result
    return fn_timer

{% endhighlight %}


Profiler decorator

{% highlight python %}

def profiler(function):
    @wraps(function)
    def fn_profile(*args, **kwargs):
        hp = hpy()
        print "Heap at the beginning of the function\n", hp.heap()
        start = time.time()
        result = function(*args, **kwargs)
        finish = time.time()
        print "Heap at the end of the function\n", hp.heap()
        print ("Total time running {0}: {1} seconds"
               .format(function.func_name, str(finish-start)))
        print '++++++++++++++++++++++++++++++++++++++++++++++'
        print
        print
        return result
    return fn_profile

{% endhighlight %}


Unix implementation

{% highlight bash %}

time -p file.py

{% endhighlight %}


Cprofile module

{% highlight bash %}

python -m cProfile -s cumulative file.py

{% endhighlight %}


Line profiler

{% highlight bash %}

pip install line_profiler
# no need to import anything, just add
@profile

kernprof -l -v file.py

{% endhighlight %}


Memory profiler

{% highlight bash %}

pip install memory_profiler
pip install psutil
@profile

python -m memory_profiler file.py

{% endhighlight %}


Grupy

{% highlight bash %}

pip install guppy
from guppy import hpy
hp = hpy()
print "Heap at the beginning of the function\n", hp.heap()
# do stuff
print "Heap at the end of the function\n", hp.heap()

python file.py

{% endhighlight %}