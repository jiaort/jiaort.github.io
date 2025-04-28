---
author: ["Runtao Jiao"]
title: "Hugo博客系统"
date: "2025-04-20"
description: "Hugo&PaperMod搭建部署博客系统"
tags: ["hugo", "papermod"]
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

<details>
<summary>deploy.yml</summary>

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

</details>

#### hugo定制化配置

##### 相册功能
https://cloud.tencent.com/developer/article/2246324

##### 博客文章封面图片缩小并移到侧边
https://cloud.tencent.com/developer/article/1969889

##### 站点统计
https://www.333rd.net/zh/posts/tech/hugo%E6%B7%BB%E5%8A%A0%E7%AB%99%E7%82%B9%E6%B5%81%E9%87%8F%E7%BB%9F%E8%AE%A1/

##### 友链
https://www.333rd.net/zh/posts/tech/hugo%E6%B7%BB%E5%8A%A0%E5%8F%8B%E9%93%BE%E9%A1%B5%E9%9D%A2/

##### 侧边目录
https://www.333rd.net/zh/posts/tech/hugo%E4%BE%A7%E8%BE%B9%E7%9B%AE%E5%BD%95/
