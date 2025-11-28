---
title: "如何配置Centos国内yum源"
summary: "这篇文章说明了如何配置centos的yum源"
keywords: "centos,yum,source"

date: 2025-11-28T19:37:49+08:00
lastmod: 2025-11-28T19:37:49+08:00

math: false
mermaid: false

categories:
  - Linux 
tags:
  - yum
---

## 1. 备份现有的 yum 源配置

首先，建议备份现有的 yum 源配置，以防出现问题可以恢复

```bash
sudo mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
```

## 2. 下载阿里云的 yum 源配置文件

使用 curl 命令从阿里云下载新的 yum 源配置文件(初始没有wget命令)

```bash
sudo curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
sudo curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

## 3. 清理缓存

下载配置文件后，需要清理 yum 缓存，以使更改生效

```bash
sudo yum clean all
```

## 4. 生成新的缓存

清理缓存后，生成新的 yum 缓存

```bash
sudo yum makecache
```

## 5. 验证更换是否成功

通过安装或更新软件包来验证是否成功更换为阿里云的 yum 源

```bash
yum repolist
```
