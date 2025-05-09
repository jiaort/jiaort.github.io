---
author: ["Runtao Jiao"]
title: "将Hugo静态网站部署到Github Pages"
date: "2025-04-20"
description: "将Hugo静态网站部署到Github Pages"
tags: ["hugo", "Github Pages"]
categories: ["环境搭建"]
autonumbering: true
weight: 1
cover:
  image: "/images/default.png"
  alt: "封面"
  relative: true
---

# Hugo静态网站部署到GitHub Pages教程

## 一、GitHub Pages简介

### 什么是GitHub Pages
GitHub Pages是一个静态站点托管服务，它直接从GitHub上的存储库获取HTML、CSS和JavaScript文件，可选地通过构建过程运行这些文件，并发布网站。

### 核心特点
- 静态内容托管：仅支持预生成的HTML、CSS、JavaScript等文件
- 自动化构建：支持通过Jekyll等静态生成器将Markdown或模板文件自动编译为静态页面
- 版本控制：依托Git管理网站内容，所有变更可追溯和回滚

### 网站类型
| 属性 | 用户和组织站点 | 项目站点 |
|------|--------------|----------|
| 源文件位置 | 必须存储在名为`<owner>.github.io`的仓库中 | 存储在项目仓库的任意文件夹中 |
| 限制 | 每个帐户最多一个页面站点 | 每个存储库最多一个页面站点 |
| 默认地址 | `https://<owner>.github.io` | `https://<owner>.github.io/<repositoryname>` |

### 使用场景
- 个人博客：结合Jekyll、Hugo等静态生成器搭建技术博客
- 项目文档：为开源项目提供在线文档
- 作品展示：设计师、开发者展示个人作品集或简历
- 在线演示：托管前端项目的实时预览页面

## 二、创建GitHub Pages站点

### 创建仓库
1. 在GitHub右上角点击"+"，选择"新建存储库"
![pages1](/images/construction/pages1.png)
2. 使用"所有者"下拉菜单选择要拥有存储库的帐户
![pages2](/images/construction/pages2.png)
3. 输入仓库名称：
   - 用户站点：必须命名为`<username>.github.io`
   - 组织站点：必须命名为`<organization>.github.io`
   - 项目站点：可以自定义名称
![pages3](/images/construction/pages3.png)

### 配置发布源
1. 进入仓库的"设置"页面
![pages4](/images/construction/pages4.png)
2. 在左侧边栏找到"Pages"
3. 在"生成和部署"部分：
   - 选择"从分支进行部署"
   - 选择发布源分支
![pages5](/images/construction/pages5.png)

## 三、配置自动化部署

### 配置访问令牌
1. 在GitHub个人设置中生成Personal Access Token
2. 在仓库的Settings > Secrets中添加名为`PERSONAL_TOKEN`的密钥
![pages6](/images/construction/pages6.png)

### 创建工作流文件
在项目根目录创建`.github/workflows/deploy.yml`文件：

```yaml
# 工作流名称
name: deploy

# 触发条件
on:
    # 当代码推送到仓库时触发
    push:
    # 允许手动触发工作流
    workflow_dispatch:

# 定义任务
jobs:
    # 构建任务
    build:
        # 在最新版本的Ubuntu系统上运行
        runs-on: ubuntu-latest
        # 定义具体步骤
        steps:
            # 步骤1：检出代码
            - name: Checkout
              uses: actions/checkout@v2
              with:
                  submodules: true
                  fetch-depth: 0

            # 步骤2：设置Hugo环境
            - name: Setup Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                  hugo-version: "latest"

            # 步骤3：构建网站
            - name: Build Web
              run: hugo

            # 步骤4：部署网站
            - name: Deploy Web
              uses: peaceiris/actions-gh-pages@v3
              with:
                  PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
                  EXTERNAL_REPOSITORY: jiaort/jiaort.github.io
                  PUBLISH_BRANCH: gh-pages
                  PUBLISH_DIR: ./public
                  commit_message: ${{ github.event.head_commit.message }}
                  keep_files: false
                  force_orphan: true
```

### 工作流说明
这个工作流程的主要作用是：
1. 当代码推送到仓库或手动触发时
2. 检出代码并设置Hugo环境
3. 使用Hugo生成静态网站
4. 将生成的静态网站部署到指定的GitHub Pages仓库
