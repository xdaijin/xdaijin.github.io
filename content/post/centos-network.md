---
title: "如何配置Centos7网络"
summary: "这篇文章介绍了安装Centos7虚拟机的时候如何设计网络"
keywords: "Centos 7 newwork"

date: 2025-11-23T02:49:50+08:00
lastmod: 2025-11-23T02:49:50+08:00

math: false
mermaid: false

categories:
  - Linux
tags:
  - network
---

## 虚拟机网络配置

虚拟机下的网络设置能联网的有两种模式

- BRIDGE
   虚拟机和宿主机共享一个网段，局域网内其他机器也能访问到虚拟机
- NAT
   虚拟机形成一个单独的局域网段，宿主机局域网内无法访问虚拟机

> 配置NAT的时候只要选择NAT模式就行了，配置BRIDGED模式的时候需要选网卡，选的网卡要和宿主机当前用的网卡一直

## Centos 系统配置

### 动态ip

1. 使用 ip addr命令查看查看网卡名和是否有网络，获知网卡名为ens33。

   ```BASH
   [xdaijin@localhost ~]$ ip addr
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
         valid_lft forever preferred_lft forever
      inet6 ::1/128 scope host 
         valid_lft forever preferred_lft forever
   2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
      link/ether 00:0c:29:de:2d:f0 brd ff:ff:ff:ff:ff:ff
      inet 192.168.228.128/24 brd 192.168.228.255 scope global noprefixroute dynamic ens33
         valid_lft 1141sec preferred_lft 1141sec
      inet6 fe80::cdd5:dd10:f5b:9cc8/64 scope link noprefixroute 
         valid_lft forever preferred_lft forever

   ```

2. 输入vi /etc/sysconfig/network-scripts/ifcfg-ens33 ，修改ifcfg-ens33配置文件

   ```BASH
   BOOTPROTO=dhcp
   ONBOOT=yes
   ```

3. 重启网络服务：

   ```BASH
   systemctl restart network
   ```

### 静态ip

1. 打开网卡配置文件：

   ```BASH
   vi /etc/sysconfig/network-scripts/ifcfg-ens32
   ```

2. 修改配置文件中的以下内容：

   ```BASH
   BOOTPROTO=static
   ONBOOT=yes
   IPADDR=192.168.1.160
   NETMASK=255.255.255.0
   GATEWAY=192.168.1.1
   DNS1=119.29.29.29
   DNS2=8.8.8.8
   ```

3. 重启网络服务：

   ```bash
   systemctl restart network
   ```

### 验证网络正常

1. 使用ip addr命令查看网络是否有ip地址

   ```bash
   [xdaijin@localhost ~]$ ip addr
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
         valid_lft forever preferred_lft forever
      inet6 ::1/128 scope host 
         valid_lft forever preferred_lft forever
   2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
      link/ether 00:0c:29:de:2d:f0 brd ff:ff:ff:ff:ff:ff
      inet 192.168.228.128/24 brd 192.168.228.255 scope global noprefixroute dynamic ens33
         valid_lft 1141sec preferred_lft 1141sec
      inet6 fe80::cdd5:dd10:f5b:9cc8/64 scope link noprefixroute 
         valid_lft forever preferred_lft forever

   ```

2. 使用ping命令检查网络是否通畅

   ```BASH
   [xdaijin@localhost ~]$ ping www.qq.com
   PING ins-r23tsuuf.ias.tencent-cloud.net (112.53.42.52) 56(84) bytes of data.
   64 bytes from 112.53.42.52 (112.53.42.52): icmp_seq=1 ttl=128 time=8.15 ms
   64 bytes from 112.53.42.52 (112.53.42.52): icmp_seq=2 ttl=128 time=7.98 ms

   ```
