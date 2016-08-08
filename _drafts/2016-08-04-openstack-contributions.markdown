---
layout: post
title:  "Making contributions to OpenStack"
date:   2016-08-04 12:30:00
categories: openstack git
---

How to backport a change from master to a stable branch

{% highlight bash %}

# Get the commit-id
git log

# Now, cherry-pick that change
git cherry-pick -x commit-id

# Push to a specific branch
git review stable/mitaka

{% endhighlight %}
