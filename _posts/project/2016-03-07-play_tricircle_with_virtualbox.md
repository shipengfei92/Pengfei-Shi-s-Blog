---
layout: post
title: Playing Tricircle with Virtualbox
category: project
description: The steps of playing tricircle with virtualbox.
---




# Playing Tricircle with Virtualbox


## 1 Preparation

1 server with Linux kernel (demo is Ubuntu 14.04 LTS)

## 2 Install softwares

### 2.1 Install virtualbox

Follow the steps in [virtualbox downloads](https://www.virtualbox.org/wiki/Linux_Downloads)

##### First, Add the following line to your /etc/apt/sources.list:
  
<br>


```
deb http://download.virtualbox.org/virtualbox/debian trusty contrib
```
According to your distribution, replace 'vivid' by 'utopic', 'trusty', 'raring', 'quantal', 'precise', 'lucid', 'jessie', 'wheezy', or 'squeeze'.

##### Then, add The Oracle public key for apt-secure:

<<<<<<< HEAD
<br>
=======
>>>>>>> 2bc3e0b41f40828a665636dcfddf51bb2dc441b3

```
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
```

##### Next, install with apt-get method:
<br>



```
sudo apt-get update
sudo apt-get install virtualbox-5.0
```


### 2.2 Connect to the virtualbox with x11


##### First, copy your public key to the ~/authorized_keys


##### Then, connect with -X command
<br>



```
ssh -X root@HostIP
```

##### Next, input the virtualbox to start install virtual machine
<br>



```
virtualbox
```
Then you can see the virtualbox graph interface:


![virtualbox](http://img.blog.csdn.net/20160307194716807)


## 3 Install Virtual Machines

For playing Tricircle, we need to install 3 nodes for devstack.One for the Top OpenStack, Two for cross pod bottom OpenStacks.

### 3.1 Configuration of VMs

#### The most important is to set up networks for use
In order to make the VMs with multiple VLAN networks, then add 2 network devices for bridge use.

##### eth0
The eth0 is the default network with NAT methods.
![eth0](http://img.blog.csdn.net/20160307194804338)


##### eth1
The eth1 is the VLAN external network,and using bridge method, in my environment, I attached to the eth1.

######[attention] The Promiscuous Mode must to set "Allow All".And be in use after reboot.
Otherwise, The Ping test with VLAN tag from Node1 to Node2 will be blocked.
![eth1](http://img.blog.csdn.net/20160307194824651)


##### eth2

The same setting as the above, the Promiscuous Mode must be set to "Allow All". And  be in use after reboot.

![eth2](http://img.blog.csdn.net/20160307194606728)

### 3.2 Installation the VMs


### 3.3 Running in the backgroud



### And more?


Follow me[@fai92](https://weibo.com/shipengfei92) on Weibo :)


[1]:    {{ page.url}}  ({{ page.title }})
