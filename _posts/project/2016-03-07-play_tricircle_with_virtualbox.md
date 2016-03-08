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
<br>

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

#### Download a ios for installation

There are many mirror sites in the world. I download a ubuntu-14.04-LTS in the [Ali-OSM](http://mirrors.aliyun.com/)(Alibaba Open Source Mirror Site). Because it's very fast in China.

#### Install the Operating System
Follow the steps while installing, because it's easy, so it will be ignored.
After installation, you will see the console like this:
![display](http://img.blog.csdn.net/20160308123435416)

### 3.3 Running in the backgroud
After installation of the VMs, we can login from SSH. So we need to let's these VMs running in the background.
And so when we close the VMs, we need to choose the option:

![background](http://img.blog.csdn.net/20160308113312422)



## 4 Playing tricircle with devstack
The detailed methods can be seen in the [OpenStack/Tricircle](https://github.com/openstack/tricircle).
### 4.1 In Top OpenStack

#### Configure the network

Install the openvswitch for creating bridges.
<br>

```
apt-get install openvswitch-switch
``` 

#### Create the stack user
<br>

```
adduser stack
``` 
Give the stack user sudo privileges:
<br>

```
apt-get install sudo -y
echo "stack ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
```
#### Download the devstack
<br>

```
sudo apt-get install git -y
git clone https://git.openstack.org/openstack-dev/devstack
cd devstack
```
 #### Configure the local.conf
Change the local.conf to fit your environment.
The example is in the Tricircle Project. such as [local.conf.sample](https://github.com/openstack/tricircle/blob/master/devstack/local.conf.sample).

#### Install the devstack with Tricircle project
<br>

```
./stack.sh
```
After installing the devstack with tricircle, the next step is verifying the installation.

#### Verifying the installation
Before verifying, It should create the client environment variables to import, such as :

admin-openrc.sh
<br>

```
export OS_PROJECT_DOMAIN_ID=default
export OS_USER_DOMAIN_ID=default
export OS_PROJECT_NAME=admin
export OS_TENANT_NAME=admin
export OS_USERNAME=admin
export OS_PASSWORD=password #change password as you set in your own environment
export OS_AUTH_URL=http://127.0.0.1:5000
export OS_IDENTITY_API_VERSION=3
#It's very important to set region name to the top openstack, because tricircle has different API urls.
export OS_REGION_NAME=RegionOne
```

Modify the verify install scripts as your own environment.
And run:
<br>

```
cd tricircle/devstack
chmod +x verify_top_install.sh
./verify_top_install.sh 2>&1 | tee logs
```
It will save the outputs in the logs, you can check if it's installed correct.

I pasted one copy in my environments as [this](http://paste.openstack.org/show/489630/).

### 4.2 Cross pods OpenStack

#### Installing

First, in the node1 install the tricircle, and then in the node2 install the tricircle.
As the above:

1 Modify the networks;
2 Create the stack user;
3 Install git;
4 Download the devstack;
5 Modify the local.conf;
6 Install the devstack with tricircle;

#### Verifying the tricircle

Before verifying, It should create the client environment variables to import, such as :

admin-openrc.sh

<br>

```
export OS_PROJECT_DOMAIN_ID=default
export OS_USER_DOMAIN_ID=default
export OS_PROJECT_NAME=admin
export OS_TENANT_NAME=admin
export OS_USERNAME=admin
export OS_PASSWORD=password #change password as you set in your own environment
export OS_AUTH_URL=http://127.0.0.1:5000
export OS_IDENTITY_API_VERSION=3
#It's very important to set region name to the top openstack, because tricircle has different API urls.
export OS_REGION_NAME=RegionOne
```

Modify the verify install scripts as your own environment.
And run:

<br>

```
cd tricircle/devstack
chmod +x verify_top_install.sh
./verify_cross_pod_install.sh 2>&1 | tee logs
```

One copy logs like [this](http://paste.openstack.org/show/489631/).

#### Ping test

Using the VNC to login the instances in Node1 and Node2.
And Ping with each other.

VM1: IP 10.0.1.3/24
<br>

```
ping -c 4 10.0.2.3
```

![vm1](http://img.blog.csdn.net/20160308143200217)

VM2: IP 10.0.2.3/24
<br>

```
ping -c 4 10.0.1.3
```

![vm2](http://img.blog.csdn.net/20160308143217405)


So the cross pod networking has been verified.



### And more?


Follow me[@fai92](https://weibo.com/shipengfei92) on Weibo :)


[1]:    {{ page.url}}  ({{ page.title }})
