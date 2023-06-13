---
dir: vagrant
title: vagrant环境搭建
date: 2022-11-06 17:21:43
tags: [Vagrant, slurm, scow]
categories: Vagrant
---

vagrant支持的provider包括virtualbox、hyperv、libvirt等。

virtualbox支持windows、linux、masoc的安装，因此本教程以virtualbox作为provider。

<!--more-->

## 1. 安装virtualbox

点击进入[官网](https://www.virtualbox.org/wiki/Downloads)下载virtualbox



![virtualbox](vagrant环境搭建/virtualbox.png)

此处可选择操作系统版本，选择Windows版本下载、安装(其他操作系统类似)。

安装过程比较简单，跟着指引即可。

## 2. 安装vagrant

点击进入[官网](https://developer.hashicorp.com/vagrant/downloads)下载vagrant

![img](vagrant环境搭建/vagrant.png)

这里选择Windows 64位版本，安装过程跟着指引即可。

## 3. vagrant基本操作

```Bash
# 新建虚拟机，创建一个centos7虚拟机

# 初始化
vagrant init centos/7

# 启动，初次启动会比较慢，需要拉镜像
vagrant up

# 查看状态
vagrant status

# ssh到虚机
 vagrant ssh
 
 # 停止虚机
 vagrant halt
 
 # 暂停虚机
 vagrant suspend
 
 # 恢复虚机
 vagrant resume
 
 # 删除虚机
 vagrant destroy
```

> 相关链接

1. virtualbox官方下载地址：https://www.virtualbox.org/wiki/Downloads
2. vagrant官方下载地址：https://developer.hashicorp.com/vagrant/downloads
3. vagrant官方文档：https://developer.hashicorp.com/vagrant/docs
4. vagrant box官方地址：https://app.vagrantup.com/boxes/search