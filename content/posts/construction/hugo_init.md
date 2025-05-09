---
author: ["Runtao Jiao"]
title: "Hugo博客系统"
date: "2025-04-20"
description: "Hugo&PaperMod搭建部署博客系统"
tags: ["hugo", "papermod"]
categories: ["环境搭建"]
autonumbering: true
cover:
  image: "/images/default.png"
  alt: "封面"
  relative: true
---

## 安装hugo

---

### MacOS安装hugo

``` shell
brew install hugo
```

### linux安装包安装hugo

``` shell
wget https://github.com/gohugoio/hugo/releases/download/v0.147.0/hugo_extended_0.147.0_Linux-64bit.tar.gz
tar -xzf hugo_extended_0.147.0_Linux-64bit.tar.gz
sudo mv hugo /usr/local/bin/
hugo version
```

### golang安装hugo

需要Go1.23.0或更高版本
``` shell
go install github.com/gohugoio/hugo@latest
```

## 创建一个hugo博客，指定yaml

```shell
hugo new site mysite --format yaml
```

## 使用git管理博客

```shell
cd mysite
git init
git branch -M main
```

## 安装hugo-PaperMod主题

```shell
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
vim hugo.yaml
theme: ["PaperMod"]
```

## 运行hugo

```shell
hugo -F --cleanDestinationDir  # 重新生成public
hugo server -D
```
---

## 我的hugo配置

``` yaml
# 全局配置
baseURL: https://jiaort.github.io     # 站点根 URL，用于生成绝对链接
title: Runtao's Blog                # 网站标题
description: 一个分享生活、阅读、学习的个人博客。  # 网站描述，用于 SEO
languageCode: zh-cn                  # 站点默认语言代码

# 主题与分页
theme: ["hugo-notice", "PaperMod"]                      # 使用的主题名称（请确保 themes 文件夹下有该主题）
pagination:
  pagerSize: 10                      # 首页每页显示的文章数

# 启用特性
hasCJKLanguage: true                 # 是否包含中日韩文字，自动优化排版
enableInlineShortcodes: true         # 启用内联 Shortcodes
enableEmoji: true                    # 允许在内容中使用 Emoji
enableRobotsTXT: true                # 自动生成 robots.txt
buildDrafts: false                   # 是否编译草稿（draft）文章
buildFuture: false                   # 是否编译将来日期的文章
buildExpired: false                  # 是否编译过期文章
pygmentsUseClasses: true             # 高亮时使用 CSS class 而不是内联样式

# 链接设置
permalinks:
  post: "/:title/"                   # 文章永久链接格式

# 语言及多语言配置
defaultContentLanguage: zh           # 默认语系
defaultContentLanguageInSubdir: true # 默认语系也生成子目录

languages:
  zh:
    languageName: "Chinese"          # 语言显示名称
    weight: 1                        # 在语言切换中的排序权重

    # 主菜单项
    menu:
      main:
        - identifier: search
          name: 🔍搜索
          url: search
          weight: 1
        - identifier: home
          name: 🏠主页
          url: /
          weight: 2
        - identifier: posts
          name: 📚文章
          url: posts
          weight: 3
        - identifier: archives
          name: ⌚时间轴
          url: archives
          weight: 20
        - identifier: categories
          name: 🏁分类
          url: categories
          weight: 30
        - identifier: tags
          name: 🔖标签
          url: tags
          weight: 40
        - identifier: about
          name: 🙋🏻‍♂️关于
          url: about
          weight: 50
        - identifier: links
          name: 🤝友链
          url: links
          weight: 60

# 输出格式
outputs:
  home:
    - HTML
    - RSS
    - JSON

# 全局参数
params:
  env: production                    # 环境标记（可用于启用特殊功能）
  homeInfoParams: false              # 关闭 homeInfoParams，启用 profileMode
  description: "这是一个纯粹的博客......"  # 描述（与顶部 description 相同，可用于模板）
  author: Runtao Jiao                # 作者
  defaultTheme: auto                 # 默认主题： light / dark / auto
  disableThemeToggle: false          # 是否禁用主题切换开关
  DateFormat: "2006-01-02"           # 全局日期格式
  ShowShareButtons: true             # 显示社交分享按钮
  ShowReadingTime: true              # 显示阅读时间
  displayFullLangName: true          # 显示完整语言名称
  ShowPostNavLinks: true             # 显示文章上下篇导航
  ShowBreadCrumbs: true              # 显示面包屑导航
  ShowCodeCopyButtons: true          # 代码块复制按钮
  hideFooter: false                  # 隐藏页脚
  ShowWordCounts: true               # 显示字数统计
  VisitCount: true                   # 显示访问计数
  ShowLastMod: true                  # 显示最后修改时间
  ShowToc: true                      # 显示文章目录
  TocOpen: true                      # 自动展开目录
  comments: true                     # 启用评论功能

  # 启用不蒜子Busuanzi流量统计
  busuanzi:
    enable: true

  # 启用fancybox图片查看器
  fancybox:
    enable: true

  # Profile 模式（首页个人简介区块）
  profileMode:
    enabled: true                  # 是否启用个人简介
    title: 生活好像不应该这样</br>但又只能这样      # 标题文字
    subtitle: "👇联系方式"  # 副标题，可含 HTML
    imageUrl: "images/tao.jpeg"    # 个人头像或展示图片路径
    imageWidth: 150                # 图片宽度（px）
    imageHeight: 150               # 图片高度（px）
    buttons:                       # 快捷按钮
      - name: 🌻生活
        url: posts/life
      - name: 📖阅读
        url: posts/read
      - name: 📝学习
        url: posts/study
      - name: 🖥环境搭建
        url: posts/construction

  # 社交图标
  socialIcons:
    - name: github
      url: "https://github.com/jiaort"
    - name: email
      url: "mailto:runtaojiao@gmail.com"
    - name: RSS
      url: "index.xml"

  # 站点图标配置
  assets:
    favicon: "favicons/favicon.ico"
    favicon16x16: "favicons/favicon.ico"
    favicon32x32: "favicons/favicon.ico"
    apple_touch_icon: "favicons/favicon.ico"
    safari_pinned_tab: "favicons/favicon.ico"

  # 搜索配置 (Fuse.js)
  fuseOpts:
    isCaseSensitive: false
    shouldSort: true
    location: 0
    distance: 1000
    threshold: 1
    minMatchCharLength: 0
    tokenize: "full"
    keys:
      - title
      - permalink
      - summary

# 分类与标签
taxonomies:
  category: categories
  tag: tags
  series: series

# Markdown 渲染配置
markup:
  goldmark:
    renderer:
      unsafe: true                   # 允许在 Markdown 中使用 HTML
  highlight:
    codeFences: true                 # 启用代码高亮围栏
    guessSyntax: true                # 猜测代码语言
    lineNos: true                    # 显示行号
    style: darcula                   # 代码高亮主题
```
