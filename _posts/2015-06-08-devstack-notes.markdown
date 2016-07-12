---
layout: post
title:  "Devstack notes"
date:   2015-06-08 10:45:00
categories: devstack
---

[Clone devstack](http://docs.openstack.org/developer/devstack/)

{% highlight bash %}
git clone https://git.openstack.org/openstack-dev/devstack
{% endhighlight %}

Make sure your vm network configuration is on bridge


set the proxies and no_proxies

{% highlight bash %}
export no_proxy=localhost,127.0.0.1,<your_ip>
{% endhighlight %}

Change connection protocol from git to https in stackrc

{% highlight bash %}
GIT_BASE=${GIT_BASE:-https://git.openstack.org}
{% endhighlight %}

If your ubuntu version is not supported run this:

{% highlight bash %}
FORCE=yes ./stack.sh
{% endhighlight %}


Create local.conf in devstack folder

{% highlight bash %}

[[local|localrc]]
#FIXED_RANGE=192.168.20.0/24
#NETWORK_GATEWAY=192.168.20.1a
LOGDAYS=1
LOGFILE=$DEST/logs/stack.sh.log
SCREEN_LOGDIR=$DEST/logs/screen
ADMIN_PASSWORD=quiet
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
SERVICE_TOKEN=a682f596-76f3-11e3-b3b2-e716f9080d50

{% endhighlight %}

Enable swift

{% highlight bash %}

SWIFT_REPLICAS=1
SWIFT_DATA_DIR=$DEST/data/swift
SWIFT_HASH=66a3d6b56c1f479c8b4e70ab5c2000f5
enable_service s-proxy s-object s-container s-account

{% endhighlight %}


Simple script to configure devstack

{% highlight bash %}

#!/usr/bin/env bash

git clone https://git.openstack.org/openstack-dev/devstack

HOST_IP=$(hostname -I)
ETH=(${HOST_IP//;/ })
ETH1 = ${ETH[1]}

export DEST=/opt/stack
export ADMIN_PASSWORD=quiet
export GIT_BASE=https://git.openstack.org

sudo cat >> /home/vagrant/devstack/local.conf << E-O-L

[[local|localrc]]
#FIXED_RANGE=192.168.20.0/24
#NETWORK_GATEWAY=192.168.20.1a
LOGDAYS=1
LOGFILE=$DEST/logs/stack.sh.log
SCREEN_LOGDIR=$DEST/logs/screen
ADMIN_PASSWORD=quiet
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
SERVICE_TOKEN=a682f596-76f3-11e3-b3b2-e716f9080d50

SWIFT_REPLICAS=1
SWIFT_DATA_DIR=$DEST/data/swift
SWIFT_HASH=66a3d6b56c1f479c8b4e70ab5c2000f5
enable_service s-proxy s-object s-container s-account

HOST_IP=$ETH1
E-O-L

cd devstack

./stack.sh

{% endhighlight %}



Set devstack with vagrant ubuntu 14.04

{% highlight bash %}

#!/usr/bin/env bash

set -x

HOST_IPS=$(hostname -I)
HOST_IPS=$(echo $HOST_IPS)
HOST_IPS=$(echo $HOST_IPS|sed 's/ /,/')

if [ "$1" == "P-R-O-X-Y" ]; then
    exit 0;
fi

# Proxy goes here
PROXY="$1"

sudo cat >> /etc/environment << E-O-L
export http_proxy=$PROXY
export https_proxy=$PROXY
export HTTP_PROXY=$PROXY
export HTTPS_PROXY=$PROXY
export no_proxy=127.0.0.1,localhost,$HOST_IPS
export NO_PROXY=127.0.0.1,localhost,$HOST_IPS
E-O-L

sudo cat > /etc/apt/apt.conf.d/01proxies << E-O-L
Acquire::http::proxy "$PROXY";
Acquire::https::proxy "$PROXY";
E-O-L

sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install git-core -y

git config --global http.proxy $PROXY
git config --global https.proxy $PROXY

exit 0;

{% endhighlight %}


Vagrantfile

{% highlight bash %}

# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|

  config.vm.box = "trusty64"

  config.vm.network "private_network", ip: "192.168.33.20"

  config.vm.provider "virtualbox" do |vb|

     vb.memory = "4096"
	   vb.cpus = 2
  end
  config.vm.provision :shell, path: "set-proxy.sh", args: "http://proxy.com:8080/", keep_color: true
  config.vm.provision :shell, path: "deploy-elasticsearch.sh", args: "", keep_color: true
  config.vm.provision :shell, path: "deploy-devstack.sh", args: "", keep_color: true

end


{% endhighlight %}
