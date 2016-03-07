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

```
deb http://download.virtualbox.org/virtualbox/debian trusty contrib
```
According to your distribution, replace 'vivid' by 'utopic', 'trusty', 'raring', 'quantal', 'precise', 'lucid', 'jessie', 'wheezy', or 'squeeze'.

##### Then, add The Oracle public key for apt-secure:

```
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
```

##### Next, install with apt-get method:

```
sudo apt-get update
sudo apt-get install virtualbox-5.0
```


### 2.2 Connect to the virtualbox with x11


##### First, copy your public key to the ~/authorized_keys


##### Then, connect with -X command

```
ssh -X root@HostIP
```

##### Next, input the virtualbox to start install virtual machine

```
virtualbox
```
Then you can see the virtualbox graph interface:

![virtualbox](https://github.com/shipengfei92/shipengfei92.github.io/blob/master/images/playTricircle/virtualbox.png)

## 3 Install Virtual Machines

For playing Tricircle, we need to install 3 nodes for devstack.One for the Top OpenStack, Two for cross pod bottom OpenStacks.

### 3.1 Configuration of VMs

#### The most important is to set up networks for use
In order to make the VMs with multiple VLAN networks, then add 2 network devices for bridge use.

##### eth0
![eth0](https://github.com/shipengfei92/shipengfei92.github.io/blob/master/images/playTricircle/eth0.png)

##### eth1

![eth1](https://github.com/shipengfei92/shipengfei92.github.io/blob/master/images/playTricircle/eth1.png)

##### eth2

![eth2](https://github.com/shipengfei92/shipengfei92.github.io/blob/master/images/playTricircle/eth2.png)



### And more?


Follow me[@fai92](https://weibo.com/shipengfei92) on Weibo :)


[1]:    {{ page.url}}  ({{ page.title }})
