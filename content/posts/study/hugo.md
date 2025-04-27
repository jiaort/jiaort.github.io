---
author: ["Runtao Jiao"]
title: "Hugo博客系统"
date: "2025-04-20"
description: "Hugo&PaperMod搭建部署博客系统"
tags: ["hugo", "papermod"]
ShowToc: true
---
#### 基础安装指南

##### 安装hugo,推荐使用安装包安装

```shell
wget https://github.com/gohugoio/hugo/releases/download/v0.147.0/hugo_extended_0.147.0_Linux-64bit.tar.gz
tar -xzf hugo_extended_0.147.0_Linux-64bit.tar.gz
sudo mv hugo /usr/local/bin/
hugo version
```

##### 创建一个hugo博客，指定yaml

```shell
hugo new site mysite --format yaml
```

##### 使用git管理博客

```shell
cd mysite
git init
git branch -M main
```

##### 安装hugo-PaperMod主题

```shell
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
vim hugo.yaml
theme: ["PaperMod"]
```

##### 运行hugo

```shell
hugo -F --cleanDestinationDir  # 重新生成public
hugo server -D
```

#### 部署hugo博客到github pages

##### 编写workflows用于打包发布

```yaml
name: deploy

on:
    push:
    workflow_dispatch:

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v2
              with:
                  submodules: true
                  fetch-depth: 0

            - name: Setup Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                  hugo-version: "latest"

            - name: Build Web
              run: hugo

            - name: Deploy Web
              uses: peaceiris/actions-gh-pages@v3
              with:
                  PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
                  EXTERNAL_REPOSITORY: yourname/yourname.github.io
                  PUBLISH_BRANCH: gh-pages  # 发布分支
                  PUBLISH_DIR: ./public
                  commit_message: ${{ github.event.head_commit.message }}
                  keep_files: false
                  force_orphan: true
```
