---
title: "Git Operation"
summary: "这篇文章介绍了git的常用操作"
keywords: "git"

date: 2025-11-24T23:24:18+08:00
lastmod: 2025-11-24T23:24:18+08:00

math: false
mermaid: false

categories:
  - 工具类
tags:
  - git
---

## 基本篇

### 创建项目

```bash
git init
或者
git clone ssh:github.com/xdaijin/my-project.git
```

### 显示当前状态

```bash
git status
```

### 将文件添加到staged状态

```bash
git add file
增加所有文件
git add .
```

### 将文件恢复到unstaged状态

```bash
git restore --staged file
```

### 显示staged的文件差异

```bash
git diff
```

### 提交

```bash
git commit -m 
```

## 日志篇

### 基本显示

```bash
git log
git log origin..HEAD
git log HEAD ^origin
```

### 单行显示

```bash
git log --oneline
```

### 图形显示

```bash
git log --oneline --graph
```

### 显示某个提交详情

```bash
git show <commit-id> 
```

## 暂存区篇

### 将当前工作目录所有tracked的文件暂存起来

```bash
git stash -m 'comment'
```

### 显示暂存区列表

```bash
git stash list
```

### 显示暂存区差异

```bash
git stash show [stash]
或者以patch形式显示
git stash show -p <stash>
```

### 使用暂存区

使用后删除：

```bash
git stash pop [stash]
```

使用后不删除：

```bash
git stash apply [stash]
```

### 废弃暂存区

```bash
git stash drop [stash]
```

### 清空暂存区

```bash
git stash clear
```

## 分支篇

### 显示分支

本地分支：

```bash
git branch
```

远程分支：

```bash
git branch -r
```

所有分支：

```bash
git branch -a
```

### 本地分支基本操作

切换：

```bash
git switch <branch>
或
git checkout <branch>
```

创建：

```bash
git switch -c <branch>
或
git branch -b <branch>
```

删除：

```bash
git branch -d <branch>
```

重命名：

```bash
git branch -m <old_branch> <new_branch>
```

### 拉取

使用fetch：

```bash
只会更新本地的origin/master，本地的master不会变化
git fetch origin master
合并到本地的master
git switch master
git merge origin/master
```

使用pull：

```bash
拉取全部
git pull origin
拉取单个分支
git pull origin master
```

### 推送

```bash
推送全部
git push origin
推送单个分支
git push origin master
```

### 合并

```bash
git merge origin/master
```

如果有冲突，解决冲突后：

```bash
git add .
git commit -m
git merge --continue
```

如果解决不了冲突，选择放弃：

```bash
git merge --abort
```

### 变基

只用于个人分支，推送个人分支前先用远程主分支做变基

```bash
git rebase origin/master
```

有个冲突解决冲突三连：

```bash
git add .
git commit -m
git rebase --continue
```

如果解决不了冲突，选择放弃：

```bash
git rebase --abort
```

### 远程分支基本操作

增加：

```bash
git remote add origin <url>
```

删除：

```bash
git remote remove origin
```

重命名：

```bash
git remote rename origin new-origin
```

查看url：

```bash
git remote get-url origin
```

修改url：

```bash
git remote set-url origin <url>
```

### 追踪远程分支

```bash
git branch -u origin
```

## 比较篇

TODO

## 回退篇

TODO

## 配置篇

TODO
