---
layout: post
title:  "Building docker clusters"
date:   2016-07-14 12:30:00
categories: docker python elasticsearch nginx go
---

The main idea is to create an elasticsearch cluster using docker and a python
web server cluster with a load balancer


# Web servers + Load balancer

## Nginx

`Dockerfile`

{% highlight bash %}

FROM nginx

COPY nginx.conf /etc/nginx/nginx.conf

{% endhighlight %}


`nginx.conf`

{% highlight bash %}

events {
  worker_connections  1024;  ## Default: 1024
}

http {
    upstream api {
        least_conn;
        # or ip_hash;
        server 192.168.1.186:9991;
        server 192.168.1.186:9992;
        server 192.168.1.186:9993;
    }

    server {
        listen 80;
        server_name 0.0.0.0;
        location / {
            proxy_pass http://api;
        }
    }
}

{% endhighlight %}

[Reference](http://nginx.org/en/docs/http/load_balancing.html)


Deploy the server as normally.


## Elasticsearch
