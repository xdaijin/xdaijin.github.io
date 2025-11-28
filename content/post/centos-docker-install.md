---
title: "如何在Centos里面安装docker"
summary: "这篇文章说明了如何在centos里面安装docker"
keywords: "centos,docker,install"

date: 2025-11-28T20:06:50+08:00
lastmod: 2025-11-28T20:06:50+08:00

math: false
mermaid: false

categories:
  - Linux
tags:
  - docker
---

## 1、卸载旧版本docker

卸载旧版本docker命令

```bash
yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine
```

## 2、安装docker的yum库

```bash
yum install -y yum-utils
```

## 3、配置docker的yum源

```bash
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

## 4、安装docker

```bash
yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 5、启动和校验docker

设置开机自动启动

```bash
sudo systemctl enable docker
```

手动启动

```bash
sudo systemctl start docker
```

重启和停止

```bash
sudo systemctl restart docker
sudo systemctl stop docker

```

验证docker是否启动

```bash
sudo docker images
```