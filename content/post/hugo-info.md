---
title: "Hugo基本概念和术语"
summary: "这是一篇介绍hugo基本概念和术语的文章"
keywords: "hugo基本概念和术语"
sticky: true
weight: 1

date: 2025-11-23T02:49:50+08:00
lastmod: 2025-11-23T02:49:50+08:00

math: false
mermaid: false

categories:
  - 前端
tags:
  - hugo
---

# hugo文件目录结构
## 网站目录结构
```
my-site/
├── archetypes/
│   └── default.md
├── assets/
├── content/
├── data/
├── i18n/
├── layouts/
├── static/
├── themes/
└── hugo.toml         <-- site configuration
```

根据需要，可以调整目录结构成下面样子：
```
my-site/
├── archetypes/
│   └── default.md
├── assets/
├── config/           <-- site configuration
│   └── _default/
│       └── hugo.toml
├── content/
├── data/
├── i18n/
├── layouts/
├── static/
└── themes/
```

编译网站后，会生成public目录和resource目录，这两个目录就是网站发布使用的目录：
```
my-site/
├── archetypes/
│   └── default.md
├── assets/
├── config/       
│   └── _default/
│       └── hugo.toml
├── content/
├── data/
├── i18n/
├── layouts/
├── public/       <-- created when you build your site
├── resources/    <-- created when you build your site
├── static/
└── themes/
```

## 目录结构解析
##### archetypes
用来管理生成content的模板，模板使用具有优先级，假如你执行下面的命令：
```
hugo new content posts/my-first-post.md
```
那么系统会按下面顺序查找模板：
1. archetypes/posts.md
2. themes/my-theme/archetypes/posts.md
3. archetypes/default.md
4. themes/my-theme/archetypes/default.md
   
##### assets
包括images, CSS, Sass, JavaScript, and TypeScript等全局资源，具体使用还待探究

##### config
需要区分环境的配置放在这个目录下面，不需要区分环境的放在根目录的hugo.toml就行了

##### content
博客的内容

##### data
数据资源，包括json，toml，yaml，xml，csv等

##### i18n
国际化配置

##### layouts
包含用来转化content、data、resources成真正网站的模板

##### static
static下面的文件会在build的时候直接copy到public目录

##### themes
主题文件
