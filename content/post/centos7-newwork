---
title: "Centos network config"
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
    虚拟机和宿主机共享一个网段，局域网内其他机器也能访问到虚拟机，需要配置静态ip
- NAT
    虚拟机只能和宿主机通讯，局域网内其他机器无法方位虚拟机，配置简单，可以dhcp

## NAT模式
在虚拟机网络配置先把网络模式设置成NAT模式

网络设置分两种方式：
- 动态ip（dhcp）
- 静态ip（static）

### 动态IP
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

### 静态IP
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
```
systemctl restart network
```

## 验证网络正常
1. 使用ip addr命令查看网络是否有ip地址
```
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

## BRIDGE模式
在虚拟机网络配置把网络模式设置成BRIDGE

按照上面静态IP设置方式设置ip，保证ip和宿主机一个网段

### 设置虚拟机网络配置
在宿主机上先看用的什么网卡:
```
以太网适配器 以太网:

   连接特定的 DNS 后缀 . . . . . . . :
   描述. . . . . . . . . . . . . . . : Intel(R) Ethernet Controller I226-V
   物理地址. . . . . . . . . . . . . : A0-AD-9F-B6-57-23
   DHCP 已启用 . . . . . . . . . . . : 是
   自动配置已启用. . . . . . . . . . : 是
   本地链接 IPv6 地址. . . . . . . . : fe80::623f:bac8:7b50:6407%17(首选)
   IPv4 地址 . . . . . . . . . . . . : 192.168.50.226(首选)
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   获得租约的时间  . . . . . . . . . : 2025年11月20日 1:02:57
   租约过期的时间  . . . . . . . . . : 2025年11月24日 16:36:50
   默认网关. . . . . . . . . . . . . : 192.168.50.1
   DHCP 服务器 . . . . . . . . . . . : 192.168.50.1
   DHCPv6 IAID . . . . . . . . . . . : 195079583
   DHCPv6 客户端 DUID  . . . . . . . : 00-01-00-01-30-AB-BE-2B-A0-AD-9F-B6-57-23
   DNS 服务器  . . . . . . . . . . . : 192.168.50.1
   TCPIP 上的 NetBIOS  . . . . . . . : 已启用
```

然后在虚拟机的菜单edit->Virtual Network Edit里面增加一个桥接的网卡，bridged to选择和宿主机一样的网卡，这点很关键