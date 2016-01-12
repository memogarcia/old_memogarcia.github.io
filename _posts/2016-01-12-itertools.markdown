---
layout: post
title:  "Python itertools"
date:   2016-01-12 19:55:11
categories: python itertools standard-library
---

This is the first post to make cheat sheets of the python standard library

Itertools
---------

{% highlight python %}

import itertools

{% endhighlight %}


Chain
=====

To combine iterables

{% highlight python %}

from itertools import chain

print list(chain([1, 2, 3], ['a', 'b', 'c']))
# [1, 2, 3, 'a', 'b', 'c']

{% endhighlight %}


izip
====

To combine elements into tuples

{% highlight python %}

from itertools import izip

print list(izip([1, 2, 3], ['a', 'b', 'c']))
# [(1, 'a'), (2, 'b'), (3, 'c')]

{% endhighlight %}


islice
======

To make slices from datasets

{% highlight python %}

from itertools import islice

for i in islice(range(10), 5):
    print i     
# 0
# 1
# 2
# 3
# 4

{% endhighlight %}


tee
===

To create multiple instances of the same iterable

{% highlight python %}

from itertools import tee

i1, i2, i3 = tee(xrange(10), 3)

{% endhighlight %}


imap
====

To map a function with a list of values

{% highlight python %}

from itertools import imap

print list(imap(lambda x: x * x, xrange(10)))
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

{% endhighlight %}


count
=====

To count objects when no ending might be possible, we define start and step.

{% highlight python %}

from itertools import count

for number, letter in izip(count(0, 10), ['a', 'b', 'c', 'd', 'e']):
    print '{0}: {1}'.format(number, letter)     
# 0: a
# 10: b
# 20: c
# 30: d
# 40: e

{% endhighlight %}


cycle
=====

To iterate over a sequence over and over until the function is stopped

{% highlight python %}

from itertools import cycle

for number, letter in izip(cycle(range(2)), ['a', 'b', 'c', 'd', 'e']):
    print '{0}: {1}'.format(number, letter)  
# 0: a
# 1: b
# 0: c
# 1: d
# 0: e

{% endhighlight %}


repeat
======

To create an iterable with items that are repeated either indefinitely or with a limit

{% highlight python %}

from itertools import cycle

for number, letter in izip(cycle(range(2)), ['a', 'b', 'c', 'd', 'e']):
    print '{0}: {1}'.format(number, letter)  
# 0: a
# 1: b
# 0: c
# 1: d
# 0: e

{% endhighlight %}


filter
======

To create a new iterable using the filter provided

{% highlight python %}

from itertools import ifilter

print list(ifilter(lambda x: x < 10, [1, 4, 6, 7, 11, 34, 66, 100, 1]))
# [1, 4, 6, 7, 1]

{% endhighlight %}


groupby
=======

To create a new iterable using the filter provided

{% highlight python %}

from operator import itemgetter
from itertools import groupby

attempts = [
    ('dan', 87),
    ('erik', 95),
    ('jason', 79),
    ('erik', 97),
    ('dan', 100)
]

# Sort the list by name for groupby
attempts.sort(key=itemgetter(0))

# Create a dictionary such that name: scores_list
print {key: sorted(map(itemgetter(1), value)) for key, value in groupby(attempts, key=itemgetter(0))}
# {'dan': [87, 100], 'jason': [79], 'erik': [95, 97]}

{% endhighlight %}
