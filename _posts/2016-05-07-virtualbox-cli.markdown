---
layout: post
title:  "[Snippet] Virtualbox CLI"
date:   2016-06-05 12:30:00
categories: Virtualbox cli configuration
---

Install linux headers

{% highlight bash %}

sudo apt-get install linux-headers-$(uname -r)

{% endhighlight %}


Download Virtualbox

{% highlight bash %}

wget http://download.virtualbox.org/virtualbox/5.0.20/VirtualBox-5.0.20-106931-Linux_amd64.run

{% endhighlight %}


Install Virtualbox

{% highlight bash %}

sudo sh VirtualBox-5.0.20-106931-Linux_amd64.run

{% endhighlight %}


Create a Virtual Drive

{% highlight bash %}

vboxmanage createmedium disk --filename hd/hlinux.vdi --size 100000 --format VDI --variant Standard

{% endhighlight %}


Check the list of OS_Types

{% highlight bash %}


VBoxManage list ostypes

{% endhighlight %}


Create the vm

{% highlight bash %}

vboxmanage createvm --name hlinux --ostype Debian_64 --uuid efb10300-2995-11e6-b5c1-0002a5d5c51b --basefolder hd --register

{% endhighlight %}


Register the vm

{% highlight bash %}

vboxmanage registervm /home/stack/.config/VirtualBox/hd/hlinux/hlinux.vbox

{% endhighlight %}


Modify the vm

{% highlight bash %}

VBoxManage storagectl efb10300-2995-11e6-b5c1-0002a5d5c51b --name "IDE Controller" --add ide

VBoxManage storageattach efb10300-2995-11e6-b5c1-0002a5d5c51b --storagectl "IDE Controller" --port 0 --device 0 --type hdd --medium hlinux.vdi

VBoxManage storageattach efb10300-2995-11e6-b5c1-0002a5d5c51b --storagectl "IDE Controller" --port 1 --device 0 --type dvddrive --medium /home/stack/Helion-Openstack-2.1.0.iso

vboxmanage modifyvm efb10300-2995-11e6-b5c1-0002a5d5c51b --memory 8096 --cpus 4

vboxmanage modifyvm efb10300-2995-11e6-b5c1-0002a5d5c51b --nic1 bridged --bridgeadapter1 eth0

{% endhighlight %}


Start the vm

{% highlight bash %}

vboxmanage startvm hlinux --type headless

{% endhighlight %}
