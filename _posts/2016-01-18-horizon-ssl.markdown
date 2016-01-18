---
layout: post
title:  "Running horizon under SSL"
date:   2016-01-18 19:55:11
categories: horizon ssl
---

Running OpenStack's horizon under SSL.


{% highlight sh %}

pip install django-extensions pyOpenSSL Werkzeug

# add django_extensions in INSTALLED_APPS in settings.py

python manage.py runserver_plus --cert cert

{% endhighlight %}
