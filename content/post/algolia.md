---
title: "如何在 hugo-theme-reimu 中使用 Algolia 搜索"
summary: "本文介绍了如何在 hugo-theme-reimu 中使用 Algolia 搜索功能，包括注册 Algolia 账号、配置 Hugo 以及上传索引的详细步骤"
keywords: "algolia"

date: 2025-11-23T03:02:38+08:00
lastmod: 2025-11-23T03:02:38+08:00

math: false
mermaid: false

categories:
  - 前端
tags:
  - algolia
---

# 注册 Algolia
1. 进入 Algolia 官网，并注册账号。
2. 进入 Dashboard - Search - Index 页面，选择上方 + Create Index 创建索引，并记住索引名。
3. 进入 Dashboard - Settings - API Keys 页面，记住如下数据：
  - Application ID
  - Search API Key
  - Admin API Key
  
# Hugo 中配置 Algolia
1. 在外层 hugo.toml 中添加如下配置，这样在执行 hugo 生成网站时同时在 public 文件夹下生成 algolia.json 文件，用于 Algolia 搜索:
```TOML
[outputs]
home = ["Algolia", "HTML", "RSS"]

[outputFormats.Algolia]
baseName = "algolia"
isPlainText = true
mediaType = "application/json"
notAlternative = true
```
2. 在 params.yml 中将 algolia_search.enable 改为 true，并填写相关信息（注意！这里填写的是Search-Only Key，不允许填写Admin Key！！否则可能被攻击
）
```YAML
algolia_search:
  enable: true
  appID: # 上文中的 Application ID
  apiKey: # 上文中的 Search API Key
  indexName: # 创建的索引名
```

# 上传索引
## 直接上传
1. 执行 hugo 生成网站，此时会在 public 文件夹下生成 algolia.json 文件。

2. 进入 Dashboard - Search - Index 页面，找到刚刚创建的索引，点击 Add records - Uploading file，选择 algolia.json 文件上传。

## 使用 atomic-algolia 包上传
使用 atomic-algolia 包来上传索引，以下步骤来自于该包的[文档](https://github.com/chrisdmacrae/atomic-algolia?tab=readme-ov-file#usage)
1. 安装 atomic-algolia 包和 cross-env 包：
   ```BASH
   npm install atomic-algolia cross-env
   ```
2. 执行 hugo 生成网站后执行如下命令
   ```BASH
   npx cross-env ALGOLIA_APP_ID={{ 上文中的 Application ID }} ALGOLIA_ADMIN_KEY={{ 上文中的 Admin API Key }} ALGOLIA_INDEX_NAME={{ 创建的索引名 }} ALGOLIA_INDEX_FILE=algolia.json atomic-algolia
   ```