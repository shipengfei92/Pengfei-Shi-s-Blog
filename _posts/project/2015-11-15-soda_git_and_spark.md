---
layout: post
title: SODA比赛之Gitlab和Spark
category: project
Description: 本文介绍了SODA比赛中Gitlab和Spark是如何提供服务的。
---


#SODA比赛


SODA比赛(Shanghai Open Data Apps)一共有超过400个参赛选手，对于比赛大数据的处理，需要强力的技术支持，我们为进入复赛的参赛队伍免费提供了两个数据平台，并分别导入大赛数据，供选手使用。它们分别是：

* Spark
* Vertica

本文介绍 Gitlab 环境的部署过程、使用方法以及如何通过后台自动调用Spark集群进行数据计算。

SODA瓶之Spark环境请戳

+ [SODA瓶之Ganglia监控](http://87boy.me/blog/2015/10/28/Ganglia.html)
+ [SODA瓶之Vertica on OpenStack](http://holysparky.github.io/2015/10/Soda-Vertica/)

##Gitlab介绍

###Gitlab

GitLab，是一个利用 Ruby on Rails 开发的开源应用程序，实现一个自托管的Git项目仓库，可通过Web界面进行访问公开的或者私人项目。

它拥有与GitHub类似的功能，能够浏览源代码，管理缺陷和注释。可以管理团队对仓库的访问，它非常易于浏览提交过的版本并提供一个文件历史库。团队成员可以利用内置的简单聊天程序（Wall）进行交流。它还提供一个代码片段收集功能可以轻松实现代码复用，便于日后有需要的时候进行查找。

###比赛代码托管平台搭建

本次比赛的所有代码考虑到隐私性、安全性等因素，采用了完全由OMNILab搭建的Gitlab私有代码仓库。参赛选手可以将编写好的代码上传至比赛专用的Gitlab，然后可以通过Gitlab调用后台为比赛专门准备的Spark计算集群，进行数据计算。

具体代码仓库搭建过程如下：

####gitlab预装环境搭建

安装包括ssh、email等服务

```
sudo apt-get install curl openssh-server ca-certificates postfix
gitlab仓库搭建以及配置
```

增加gitlab的package并且安装：

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo apt-get install gitlab-ce
```
####配置并启动gitlab服务
```
curl -LJO https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/trusty/gitlab-ce-XXX.deb/download
dpkg -i gitlab-ce-XXX.deb
```
####批量创建用户名、密码

首先，通过正则表达式匹配，将KESCI提供的邮箱中的用户名进行提取：

```
cat list.txt | sed 's/\(.*\)@\(.*\)/\1/g' >>user.list
```

分别对应生成密码，这里采用了linux下的makepassword工具包批量生成密码

```
makepasswd --count=404
```

采用gitlab的命令行工具进行批量的用户创建

gitlab 提供了一个可以通过private token的方式进行命令行创建用户等操作，这样省去了手动创建的操作。

我们通过以下两个脚本进行用户的批量创建：

* 创建用户脚本

```
#!/bin/bash

#MAIL=$1
USER=$1
PASS=$2

curl -H "Content-Type:application/json" http://IP/api/v3/users?private_token=XXXXXXX -d \
"{ \"email\":\"$1@IP\",\"password\":\"$2\",\"name\":\"$1\",\"username\":\"$1\",\"confirm\":\"false\" }"

```

* 批量创建脚本

```
#!/bin/bash

cat final.txt | while read name password
do
  echo $name
  echo $password
  ./create-git-account.sh $name $password
done
```

##Spark介绍

###Spark

Apache Spark是一个开源簇运算框架，最初是由加州大学柏克莱分校AMPLab所开发。相对于Hadoop的MapReduce会在运行完工作后将中介数据存放到磁盘中，Spark使用了存储器内运算技术，能在数据尚未写入硬盘时即在存储器内分析运算。Spark在存储器内运行程序的运算速度能做到比Hadoop MapReduce的运算速度快上100倍，即便是运行程序于硬盘时，Spark也能快上10倍速度。

###Spark集群介绍

本次比赛中OMNILab为参赛选手提供了强大的Spark集群：

* 2个控制节点，8个计算节点，1个客户端节点
* 共计256核和2TB物理内存可用于计算

###使用方法

* 上传代码到git,参照模版来提交你的代码，代码准备完毕后创建release branch
后台调度器会自动调度代码到Spark上运行
* 从release branch上拿到结果
* 搞定！




### And more?


Follow me[@fai92](https://weibo.com/shipengfei92) on Weibo :)


[1]:    {{ page.url}}  ({{ page.title }})
