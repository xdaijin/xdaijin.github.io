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

## 文件目录管理

```text
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

```text
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

```text
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

### archetypes

用来管理生成content的模板，模板使用具有优先级，假如你执行下面的命令：

```bash
hugo new content posts/my-first-post.md
```

那么系统会按下面顺序查找模板：

1. archetypes/posts.md
2. themes/my-theme/archetypes/posts.md
3. archetypes/default.md
4. themes/my-theme/archetypes/default.md

### assets

包括images, CSS, Sass, JavaScript, and TypeScript等全局资源，具体使用还待探究

### config

需要区分环境的配置放在这个目录下面，不需要区分环境的放在根目录的hugo.toml就行了

### content

博客的内容

### data

数据资源，包括json，toml，yaml，xml，csv等

### i18n

国际化配置

### layouts

包含用来转化content、data、resources成真正网站的模板

### static

static下面的文件会在build的时候直接copy到public目录

### themes

主题文件

## 内容管理

TODO

## URL管理

hugo生成网站的时候，内容会安装下面格式输出：

```text
url ("/posts/my-first-hugo-post/")
                   ⊢------------^----------⊣
       baseurl     section     slug
⊢--------^--------⊣⊢-^--⊣⊢-------^---------⊣
                 permalink
⊢--------------------^---------------------⊣
https://example.org/posts/my-first-hugo-post/index.html
```

### section

content下面带_index.md的子目录，/posts/my-first-hugo-post里面的posts

### slug

路径的最后一段，可以在front matter里面配置，例如：

```text
+++
slug = 'my-first-post'
title = 'My First Post'
+++
```
