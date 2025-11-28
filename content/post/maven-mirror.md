---
title: "如何配置Maven阿里云国内镜像"
summary: "maven阿里云镜像配置文档"
keywords: "maven,mirror"

date: 2025-11-28T22:12:51+08:00
lastmod: 2025-11-28T22:12:51+08:00

math: false
mermaid: false

categories:
  - Java
tags:
  - maven
---

## 打开setting文件

setting文件有两个：

- 安装目录下，修改对所有用户生效
- home/.m2目录下，修改对当前用户生效

## 增加Mirror配置

在mirrors里面增加下面配置：

```xml
<mirror>
  <id>aliyun-central</id>
  <mirrorOf>central</mirrorOf>
  <name>Aliyun central</name>
  <url>https://maven.aliyun.com/repository/central</url>
</mirror>

<mirror>
  <id>aliyun-public</id>
  <mirrorOf>public</mirrorOf>
  <name>Aliyun public</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>

<mirror>
  <id>aliyun-gradle-plugin</id>
  <mirrorOf>gradle-plugin</mirrorOf>
  <name>Aliyun gradle plugin</name>
  <url>https://maven.aliyun.com/repository/gradle-plugin</url>
</mirror>

<mirror>
  <id>aliyun-apache-snapshots</id>
  <mirrorOf>apache snapshots</mirrorOf>
  <name>Aliyun apache snapshots</name>
  <url>https://maven.aliyun.com/repository/apache-snapshots</url>
</mirror>
```

配置说明：

1. id：唯一标识符
2. mirrorOf：对哪个仓库做镜像，如果没有别的仓库，只用对中央仓库进行镜像
3. name：镜像名称
4. url：镜像url

## 验证

```bash
mvn help:effective-settings
```
