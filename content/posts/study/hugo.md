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

## 基础安装指南

### 安装hugo,推荐使用安装包安装

```shell
wget https://github.com/gohugoio/hugo/releases/download/v0.147.0/hugo_extended_0.147.0_Linux-64bit.tar.gz
tar -xzf hugo_extended_0.147.0_Linux-64bit.tar.gz
sudo mv hugo /usr/local/bin/
hugo version
```

### 创建一个hugo博客，指定yaml

```shell
hugo new site mysite --format yaml
```

### 使用git管理博客

```shell
cd mysite
git init
git branch -M main
```

### 安装hugo-PaperMod主题

```shell
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
vim hugo.yaml
theme: ["PaperMod"]
```

### 运行hugo

```shell
hugo -F --cleanDestinationDir  # 重新生成public
hugo server -D
```
---

### 我的hugo配置

<details>
<summary>hugo.yaml</summary>

``` yaml
# 全局配置
baseURL: https://jiaort.github.io     # 站点根 URL，用于生成绝对链接
title: Runtao's Blog                # 网站标题
description: 一个分享生活、阅读、学习的个人博客。  # 网站描述，用于 SEO
languageCode: zh-cn                  # 站点默认语言代码

# 主题与分页
theme: PaperMod                      # 使用的主题名称（请确保 themes 文件夹下有该主题）
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
      - name: 📚学习
        url: posts/study

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

</details>

---

## 部署hugo博客到github pages

### 创建一个Giithub pages仓库

创建一个命名为`yourname.github.io`的仓库，并开启Github pages功能。

### 提交hugo代码

```shell
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/crazy-dogg/crazy-dogg.github.io.git
git push -u origin main
```

### 编写workflows用于打包发布
配置`PERSONAL_TOKEN`环境变量，用于发布到Github pages仓库。
![PERSONAL_TOKEN](/images/study/secret_variables.png)

创建并选择发布仓库的分支为`gh-pages`
![gh-pages](/images/study/gh-pages.png)]

编写workflows用于打包发布，在`.github/workflows`目录下创建`deploy.yml`文件，内容如下：

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

---

## hugo定制化配置

### 添加评论功能
参考giscus官网： [评论插件](https://giscus.app/zh-CN)

> vim layouts/partials/comments.html

```html
<script src="https://giscus.app/client.js"
        data-repo="[在此输入仓库]"
        data-repo-id="[在此输入仓库 ID]"
        data-category="[在此输入分类名]"
        data-category-id="[在此输入分类 ID]"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="zh-CN"
        crossorigin="anonymous"
        async>
</script>
```

### 相册功能

**step1**
> vim layouts/shortcodes/galleries.html
<details>
<summary>galleries.html</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1, maximum-scale=1">
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/nanogallery2@3.0.5/dist/css/nanogallery2.min.css">
    <script src="https://cdn.jsdelivr.net/npm/nanogallery2@3.0.5/dist/jquery.nanogallery2.min.js"></script>
</head>
<body>

<div data-nanogallery2='{
	 "thumbnailDisplayTransition":          "none",
     "thumbnailDisplayTransitionDuration":  500,
     "thumbnailDisplayInterval":            30,
     "galleryDisplayTransition":            "none",
     "galleryDisplayTransitionDuration":    500,
     "galleryDisplayMode": "rows",
     "thumbnailDisplayOutsideScreen": "false",
     "eventsDebounceDelay": 10,
     "thumbnailL1BorderHorizontal": 0,
     "thumbnailL1BorderVertical": 0,
     "thumbnailLabel": {
        "titleFontSize": "0.6em"
     },
     "thumbnailHoverEffect2": "image_scale_1.00_1.10|label_backgroundColor_rgba(0,0,0,0)_rgba(255,255,255,0)",
     "galleryTheme": {
        "thumbnail": {
            "borderRadius": "8px"
        }
     },
     "thumbnailToolbarImage": {
        "topLeft": "",
        "topRight": "",
        "bottomLeft": "",
        "bottomRight": ""
     },
     "viewerToolbar":   {
        "display": true,
        "standard": "label"
     },
     "viewerTools":     {
        "topLeft":    "pageCounter, playPauseButton",
        "topRight":   "downloadButton, rotateLeft, zoomButton, fullscreenButton, closeButton"
     },
     "viewerGalleryTWidth": 40,
     "viewerGalleryTHeight": 40
}'>
    {{ .Inner }}
</div>
</body>
</html>
```

</details>

**step2**
> vim layouts/shortcodes/gallery.html

```html
<a href="{{ .Get "src" }}" data-ngThumb="{{ .Get "src" }}">{{ .Get "title" }}</a>

```

**step3**
> 使用方法:使用前需要将`.`去掉
```shell
{.{< galleries >}}
{.{< gallery src="/images/life/qinghai/2.jpg" title="青海湖骑行">}}
{.{< /galleries >}}
```

### 博客文章封面图片缩小并移到侧边

> vim layouts/_default/list.html
> 
> 替换article标签

```html
<article class="{{ $class }}">
  <div class="post-info">  <!--新添加的行-->
    <header class="entry-header">
        <h2>
            {{- .Title }}
            {{- if .Draft }}<sup><span class="entry-isdraft">&nbsp;&nbsp;[draft]</span></sup>{{- end }}
        </h2>
    </header>
    {{- if (ne (.Param "hideSummary") true) }}
    <section class="entry-content">
        <p>{{ .Summary | plainify | htmlUnescape }}{{ if .Truncated }}...{{ end }}</p>
    </section>
    {{- end }}
    {{- if not (.Param "hideMeta") }}
    <footer class="entry-footer">
        {{- partial "post_meta.html" . -}}
    </footer>
    {{- end }}
  </div>  <!--新添加的行-->
  <a class="entry-link" aria-label="post link to {{ .Title | plainify }}" href="{{ .Permalink }}"></a>
  {{- $isHidden := (.Param "cover.hiddenInList") | default (.Param "cover.hidden") | default false }}
  {{- partial "cover.html" (dict "cxt" . "IsHome" true "isHidden" $isHidden)
</article>
```

> vim assets/css/extended/blank.css
> 
> 配置css

```css
/* 博客文章封面图片缩小并移到侧边 */
.entry-cover1 {
    border-radius: 10px;
    display: flex;
    justify-content: center;
}

.post-entry {
    display: flex;
    flex-direction: row;
    align-items: center;
}

.entry-cover {
    overflow: hidden;
    padding-left: 18px;
    height: 100%;
    width: 30%;
    margin-bottom: unset;
}

.post-info {
    display: inline-block;
    overflow: hidden;
    width: 90%;
}
```


> vim layouts/_default/single.html
> 
> 配置cover1显示，确保不改变内容中图片的显示

```html
  {{- $isHidden := (.Param "cover.hiddenInSingle") | default (.Param "cover.hidden") | default false }}
  {{- partial "cover1.html" (dict "cxt" . "IsSingle" true "isHidden" $isHidden) }}
  {{- if (.Param "ShowToc") }}
  {{- partial "toc.html" . }}
  {{- end }}
```

### 站点统计

**step1**
> vim layouts/partials/extend_head.html
> 
> 头部引入busuanzi流量统计，追加代码

```html
<!-- 不蒜子Busuanzi流量统计 -->
{{- if .Site.Params.busuanzi.enable -}}
  <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
  <meta name="referrer" content="no-referrer-when-downgrade">
{{- end -}}
```

**step2**
> vim layouts/partials/extend_footer.html
> 
> 底部引入busuanzi流量统计，追加代码

```html
<!-- 不蒜子Busuanzi流量统计，站点底部显示总访问量与访客数 -->
{{ if .Site.Params.busuanzi.enable -}}
<div class="busuanzi-footer" style="font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; text-align: center; padding: 4px 0; color: #999;">
  <span id="busuanzi_container_site_pv" style="margin-right: 8px; font-size: 0.85em;">
    本站总访问量<span id="busuanzi_value_site_pv" style="font-weight: 500;">0</span>次
  </span>
  <span id="busuanzi_container_site_uv" style="font-size: 0.85em;">
    本站访客数<span id="busuanzi_value_site_uv" style="font-weight: 500;">0</span>人次
  </span>
</div>
{{- end -}}
```

**step3**
> vim layouts/partials/_default/single.html
> 
> 文章引入busuanzi流量统计，在固定位置添加

```html
    {{- if not (.Param "hideMeta") }}
    <div class="post-meta">
      {{- partial "post_meta.html" . -}}
      {{- partial "translation_list.html" . -}}
      {{- partial "edit_post.html" . -}}
      {{- partial "post_canonical.html" . -}}
      <!-- 不蒜子Busuanzi流量统计 -->
      {{ if .Site.Params.busuanzi.enable -}}
      <div class="meta-busuanzi">&nbsp·&nbsp
        <span id="busuanzi_container_page_pv">本文阅读量<span id="busuanzi_value_page_pv">0</span>次</span>
      </div>
      {{- end }}
    </div>
    {{- end }}
```

**step4**
> vim hugo.yaml
> 
> 在配置中启用busuanzi

```yaml
params:  
    busuanzi:
        enable: true # 启用不蒜子Busuanzi流量统计
```


### 友链

**step1**
> vim layouts/shortcodes/friendlink.html
>
> 定义短代码

```html
<article class="friend-card">
  <a class="friend-link" href="{{ .Get "url" }}" target="_blank">
    <div class="friend-logo">
      <img src="{{ .Get "logo" }}" alt="友链LOGO" loading="lazy" onerror="this.onerror=null; this.src='/images/male.png';">
    </div>
    <div class="friend-info">
      <div class="friend-name">{{ .Get "name" }}</div>
      <div class="friend-slogan">{{ .Get "slogan" }}</div>
    </div>
  </a>
</article>
```

**step2**
> vim assets/css/extended/link.css
>
> 添加样式

<details>
<summary>list.html</summary>

```css
.friend-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  margin: 20px 0;
}

.friend-card {
  flex: 0 1 200px;
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  min-height: 250px; /* 确保卡片高度一致 */
}

.friend-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}

.friend-link {
  display: block;
  text-decoration: none;
  color: var(--content);
  padding: 15px;
  text-align: center;
}

.friend-logo {
  width: 100%;
  height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f7f7f7;
}

.friend-logo img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  transition: opacity 0.3s ease;
}

.friend-logo:hover img {
  opacity: 0.8;
}

.friend-info {
  padding: 10px;
  text-align: center;
  min-height: 100px; /* 确保有足够空间容纳slogan */
}

.friend-name {
  font-size: 18px;
  font-weight: bold;
  margin: 5px 0;
}

.friend-slogan {
  font-size: 14px;
  color: #666;
  margin: 0;
}

@media (max-width: 768px) {
  .friend-card {
    flex: 0 1 150px;
  }
  
  .friend-logo {
    height: 120px;
  }
  
  .friend-name {
    font-size: 16px;
  }
  
  .friend-slogan {
    font-size: 12px;
  }
}
```

</details>

**step3**
> 使用方法:使用前需要将`.`去掉

```html
<div class="friend-container">
{.{< friendlink name="" url="" logo="" slogan="一个Python程序员的博客" >}}
</div>

```

### 侧边目录

**step1**
> vim layouts/partials/toc.html

<details>
<summary>toc.html</summary>

```html
{{- $headers := findRE "<h[1-6].*?>(.|\n])+?</h[1-6]>" .Content -}}
{{- $has_headers := ge (len $headers) 1 -}}
{{- if $has_headers -}}
<aside id="toc-container" class="toc-container wide">
    <div class="toc">
        <details {{if (.Param "TocOpen") }} open{{ end }}>
            <summary accesskey="c" title="(Alt + C)">
                <span class="details">{{- i18n "toc" | default "Table of Contents" }}</span>
            </summary>

            <div class="inner">
                {{- $largest := 6 -}}
                {{- range $headers -}}
                {{- $headerLevel := index (findRE "[1-6]" . 1) 0 -}}
                {{- $headerLevel := len (seq $headerLevel) -}}
                {{- if lt $headerLevel $largest -}}
                {{- $largest = $headerLevel -}}
                {{- end -}}
                {{- end -}}

                {{- $firstHeaderLevel := len (seq (index (findRE "[1-6]" (index $headers 0) 1) 0)) -}}

                {{- $.Scratch.Set "bareul" slice -}}
                <ul>
                    {{- range seq (sub $firstHeaderLevel $largest) -}}
                    <ul>
                        {{- $.Scratch.Add "bareul" (sub (add $largest .) 1) -}}
                        {{- end -}}
                        {{- range $i, $header := $headers -}}
                        {{- $headerLevel := index (findRE "[1-6]" . 1) 0 -}}
                        {{- $headerLevel := len (seq $headerLevel) -}}

                        {{/* get id="xyz" */}}
                        {{- $id := index (findRE "(id=\"(.*?)\")" $header 9) 0 }}

                        {{- /* strip id="" to leave xyz, no way to get regex capturing groups in hugo */ -}}
                        {{- $cleanedID := replace (replace $id "id=\"" "") "\"" "" }}
                        {{- $header := replaceRE "<h[1-6].*?>((.|\n])+?)</h[1-6]>" "$1" $header -}}

                        {{- if ne $i 0 -}}
                        {{- $prevHeaderLevel := index (findRE "[1-6]" (index $headers (sub $i 1)) 1) 0 -}}
                        {{- $prevHeaderLevel := len (seq $prevHeaderLevel) -}}
                        {{- if gt $headerLevel $prevHeaderLevel -}}
                        {{- range seq $prevHeaderLevel (sub $headerLevel 1) -}}
                        <ul>
                            {{/* the first should not be recorded */}}
                            {{- if ne $prevHeaderLevel . -}}
                            {{- $.Scratch.Add "bareul" . -}}
                            {{- end -}}
                            {{- end -}}
                            {{- else -}}
                            </li>
                            {{- if lt $headerLevel $prevHeaderLevel -}}
                            {{- range seq (sub $prevHeaderLevel 1) -1 $headerLevel -}}
                            {{- if in ($.Scratch.Get "bareul") . -}}
                        </ul>
                        {{/* manually do pop item */}}
                        {{- $tmp := $.Scratch.Get "bareul" -}}
                        {{- $.Scratch.Delete "bareul" -}}
                        {{- $.Scratch.Set "bareul" slice}}
                        {{- range seq (sub (len $tmp) 1) -}}
                        {{- $.Scratch.Add "bareul" (index $tmp (sub . 1)) -}}
                        {{- end -}}
                        {{- else -}}
                    </ul>
                    </li>
                    {{- end -}}
                    {{- end -}}
                    {{- end -}}
                    {{- end }}
                    <li>
                        <a href="#{{- $cleanedID -}}" aria-label="{{- $header | plainify -}}">{{- $header | safeHTML -}}</a>
                        {{- else }}
                    <li>
                        <a href="#{{- $cleanedID -}}" aria-label="{{- $header | plainify -}}">{{- $header | safeHTML -}}</a>
                        {{- end -}}
                        {{- end -}}
                        <!-- {{- $firstHeaderLevel := len (seq (index (findRE "[1-6]" (index $headers 0) 1) 0)) -}} -->
                        {{- $firstHeaderLevel := $largest }}
                        {{- $lastHeaderLevel := len (seq (index (findRE "[1-6]" (index $headers (sub (len $headers) 1)) 1) 0)) }}
                    </li>
                    {{- range seq (sub $lastHeaderLevel $firstHeaderLevel) -}}
                    {{- if in ($.Scratch.Get "bareul") (add . $firstHeaderLevel) }}
                </ul>
                {{- else }}
                </ul>
                </li>
                {{- end -}}
                {{- end }}
                </ul>
            </div>
        </details>
    </div>
</aside>
<script>
    let activeElement;
    let elements;
    window.addEventListener('DOMContentLoaded', function (event) {
        checkTocPosition();

        elements = document.querySelectorAll('h1[id],h2[id],h3[id],h4[id],h5[id],h6[id]');
         // Make the first header active
         activeElement = elements[0];
         const id = encodeURI(activeElement.getAttribute('id')).toLowerCase();
         document.querySelector(`.inner ul li a[href="#${id}"]`).classList.add('active');
     }, false);

    window.addEventListener('resize', function(event) {
        checkTocPosition();
    }, false);

    window.addEventListener('scroll', () => {
        // Check if there is an object in the top half of the screen or keep the last item active
        activeElement = Array.from(elements).find((element) => {
            if ((getOffsetTop(element) - window.pageYOffset) > 0 && 
                (getOffsetTop(element) - window.pageYOffset) < window.innerHeight/2) {
                return element;
            }
        }) || activeElement

        elements.forEach(element => {
             const id = encodeURI(element.getAttribute('id')).toLowerCase();
             if (element === activeElement){
                 document.querySelector(`.inner ul li a[href="#${id}"]`).classList.add('active');
             } else {
                 document.querySelector(`.inner ul li a[href="#${id}"]`).classList.remove('active');
             }
         })
     }, false);

    const main = parseInt(getComputedStyle(document.body).getPropertyValue('--article-width'), 10);
    const toc = parseInt(getComputedStyle(document.body).getPropertyValue('--toc-width'), 10);
    const gap = parseInt(getComputedStyle(document.body).getPropertyValue('--gap'), 10);

    function checkTocPosition() {
        const width = document.body.scrollWidth;

        if (width - main - (toc * 2) - (gap * 4) > 0) {
            document.getElementById("toc-container").classList.add("wide");
        } else {
            document.getElementById("toc-container").classList.remove("wide");
        }
    }

    function getOffsetTop(element) {
        if (!element.getClientRects().length) {
            return 0;
        }
        let rect = element.getBoundingClientRect();
        let win = element.ownerDocument.defaultView;
        return rect.top + win.pageYOffset;   
    }
</script>
{{- end }}
```

</details>

**step2**
> vim assets/css/extended/blank.css

<details>
<summary>blank.css</summary>

```css
:root {
    --nav-width: 1380px;
    --article-width: 650px;
    --toc-width: 300px;
}

.toc {
    margin: 0 2px 40px 2px;
    border: 1px solid var(--border);
    background: var(--entry);
    border-radius: var(--radius);
    padding: 0.4em;
}

.toc-container.wide {
    position: absolute;
    height: 100%;
    border-right: 1px solid var(--border);
    left: calc((var(--toc-width) + var(--gap)) * -1);
    top: calc(var(--gap) * 2);
    width: var(--toc-width);
}

.wide .toc {
    position: sticky;
    top: var(--gap);
    border: unset;
    background: unset;
    border-radius: unset;
    width: 100%;
    margin: 0 2px 40px 2px;
}

.toc details summary {
    cursor: zoom-in;
    margin-inline-start: 20px;
    padding: 12px 0;
}

.toc details[open] summary {
    font-weight: 500;
}

.toc-container.wide .toc .inner {
    margin: 0;
}

.active {
    font-size: 110%;
    font-weight: 600;
}

.toc ul {
    list-style-type: circle;
}

.toc .inner {
    margin: 0 0 0 20px;
    padding: 0px 15px 15px 20px;
    font-size: 16px;
}

.toc li ul {
    margin-inline-start: calc(var(--gap) * 0.5);
    list-style-type: none;
}

.toc li {
    list-style: none;
    font-size: 0.95rem;
    padding-bottom: 5px;
}

.toc li a:hover {
    color: var(--secondary);
}
```

</details>


### 字体居中

```shell
vim layouts/shortcodes/center.html
<div style="text-align: center;">
{{ .Inner }}
</div>
```
> 示例 去掉'/'

``` shell
{/{% center %}}
陈奕迅 - 无条件
{/{% /center %}}
```
### 标签美化

> vim assets/css/extended/blank.css

```css

/*标签*/
.terms-tags {
    text-align: center;
}

.terms-tags a:hover {
    background: none;

    webkit-transform: scale(1.2);
    -moz-transform: scale(1.2);
    -ms-transform: scale(1.2);
    -o-transform: scale(1.2);
    transform: scale(1.3);
}

.terms-tags a {
    border-radius: 30px;
    background: none;
    transition: transform 0.5s;
}

.dark .terms-tags a {
    background: none;
}

.dark .terms-tags a:hover {
    background: none;

    webkit-transform: scale(1.2);
    -moz-transform: scale(1.2);
    -ms-transform: scale(1.2);
    -o-transform: scale(1.2);
    transform: scale(1.3);
}

.terms-tags li {
    margin: 5px;
}

```

### latex 数学公式

> vim layouts/partials/mathjax.html

``` html
<script type="text/javascript"
        async
        src="https://cdn.bootcss.com/mathjax/2.7.3/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[\[','\]\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});

MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>

<style>
code.has-jax {
    font: inherit;
    font-size: 100%;
    background: inherit;
    border: inherit;
    color: #515151;
}
</style>
```

> vim layouts/partials/extend_head.html
>
> 导入数学公式支持html

``` html
{{ partial "mathjax.html" . }}
```

**latex 语法**
``` shell
$$ \boldsymbol{x}{i+1}+\boldsymbol{x}{i+2} $$
```
**效果**
$$ \boldsymbol{x}{i+1}+\boldsymbol{x}{i+2} $$

### mermaid 流程图

> vim hugo.yaml

``` shell
  # Mermaid 流程图支持
  mermaid:
    enable: true
```

> vim layouts/partials/extend_head.html

``` html
<!-- Mermaid 流程图支持 -->
{{ if (.Site.Params.mermaid.enable) }}
<script defer src="https://unpkg.com/mermaid@8.8.1/dist/mermaid.min.js"></script>
{{ end }}
```

> vim layouts/shortcodes/mermaid.html
> 使用代码块

``` html
<div class="mermaid">{{.Inner}}</div>
```

> vim assets/css/extended/blank.css
``` css
/* 让流程图居中 */
.mermaid {
    display: flex;
    justify-content: center;
    margin: 10px 0px 25px 0px
}
```

**流程图示例**
{{< mermaid >}}
flowchart LR
    a --> b & c --> d
{{< /mermaid >}}

### 图片放大
自动适配markdown图片，只需将一下文件写入即可

> vim hugo.yaml 开启fancybox

``` yaml
  # 启用fancybox图片查看器
  fancybox:
    enable: true
```

> vim layouts/partials/extend_head.html

``` html
{{- if .Page.Site.Params.fancybox.enable -}}
<script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.css" />
<script src="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.js"></script>
{{- end -}}
```

> vim 替换 layouts/_default/_markup/render-image.html

``` html
{{if .Page.Site.Params.fancybox.enable }}
<div class="post-img-view">
<a data-fancybox="gallery" href="{{ .Destination | safeURL }}">
<img src="{{ .Destination | safeURL }}" alt="{{ .Text }}" {{ with .Title}} title="{{ . }}"{{ end }} />
</a>
</div>
{{ end }}
```

### 增加notice块

参考：[hugo-notice网站](https://github.com/martignoni/hugo-notice)

{{< notice info >}}
创建代码块, vim layouts/shortcodes/notice.html
{{< /notice >}}

<details>
<summary>notice.html</summary>

``` html
{{/* Available notice types: warning, info, note, tip */}}
{{- $noticeType := .Get 0 | default "note" -}}

{{/* Workaround markdownify inconsistency for single/multiple paragraphs */}}
{{- $raw := (markdownify .Inner | chomp) -}}
{{- $block := findRE "(?is)^<(?:address|article|aside|blockquote|canvas|dd|div|dl|dt|fieldset|figcaption|figure|footer|form|h(?:1|2|3|4|5|6)|header|hgroup|hr|li|main|nav|noscript|ol|output|p|pre|section|table|tfoot|ul|video)\\b" $raw 1 -}}

{{/* Load the css if it's the first time */}}
{{- if not (.Page.Store.Get "notice-style-loaded-flag") -}}
<style type="text/css">
    /* Light theme */
    .notice {
        --title-color: #fff;
        --title-background-color: #6be;
        --content-color: #444;
        --content-background-color: #e7f2fa;
    }

    .notice.info {
        --title-background-color: #fb7;
        --content-background-color: #fec;
    }

    .notice.tip {
        --title-background-color: #5a5;
        --content-background-color: #efe;
    }

    .notice.warning {
        --title-background-color: #c33;
        --content-background-color: #fee;
    }

    /* Dark theme */
    /* @media (prefers-color-scheme:dark) {
        .notice {
            --title-color: #fff;
            --title-background-color: #069;
            --content-color: #ddd;
            --content-background-color: #023;
        }

        .notice.info {
            --title-background-color: #a50;
            --content-background-color: #420;
        }

        .notice.tip {
            --title-background-color: #363;
            --content-background-color: #121;
        }

        .notice.warning {
            --title-background-color: #800;
            --content-background-color: #400;
        }
    } */

    body.dark .notice {
        --title-color: #fff;
        --title-background-color: #069;
        --content-color: #ddd;
        --content-background-color: #023;
    }

    body.dark .notice.info {
        --title-background-color: #a50;
        --content-background-color: #420;
    }

    body.dark .notice.tip {
        --title-background-color: #363;
        --content-background-color: #121;
    }

    body.dark .notice.warning {
        --title-background-color: #800;
        --content-background-color: #400;
    }

    /* Content */
    .notice {
        padding: 12px;
        line-height: 18px;
        margin-bottom: 20px;
        border-radius: 4px;
        color: var(--content-color);
        background: var(--content-background-color);
        display: flex;
        align-items: flex-start;
    }

    .notice p:last-child {
        margin-bottom: 0
    }

    /* Icon */
    .icon-notice {
        display: inline-flex;
        align-self: flex-start;
        margin-right: 12px;
        flex-shrink: 0;
    }

    .icon-notice img,
    .icon-notice svg {
        height: 1.0em;
        width: 1.0em;
        fill: currentColor;
    }

    .notice-content {
        flex: 1;
    }
</style>
{{- .Page.Store.Set "notice-style-loaded-flag" true -}}
{{- end -}}

<div class="notice {{ $noticeType }}" {{ if len .Params | eq 2 }} id="{{ .Get 1 }}" {{ end }}>
    <span class="icon-notice">
        {{ printf "static/icons/%s.svg" $noticeType | readFile | safeHTML }}
    </span>
    <div class="notice-content">
        {{- if or $block (not $raw) }}{{ $raw }}{{ else }}<p>{{ $raw }}</p>{{ end -}}
    </div>
</div>
```

</details>


{{< notice note >}}
创建icons文件夹 分别放入不同等级svg mkdir -p static/icons
vim info.svg、note.svg、tips.svg、warning.svg
{{< /notice >}}

``` html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="92 59.5 300 300">
  <path d="M292 303.25V272c0-3.516-2.734-6.25-6.25-6.25H267v-100c0-3.516-2.734-6.25-6.25-6.25h-62.5c-3.516 0-6.25 2.734-6.25 6.25V197c0 3.516 2.734 6.25 6.25 6.25H217v62.5h-18.75c-3.516 0-6.25 2.734-6.25 6.25v31.25c0 3.516 2.734 6.25 6.25 6.25h87.5c3.516 0 6.25-2.734 6.25-6.25Zm-25-175V97c0-3.516-2.734-6.25-6.25-6.25h-37.5c-3.516 0-6.25 2.734-6.25 6.25v31.25c0 3.516 2.734 6.25 6.25 6.25h37.5c3.516 0 6.25-2.734 6.25-6.25Zm125 81.25c0 82.813-67.188 150-150 150-82.813 0-150-67.188-150-150 0-82.813 67.188-150 150-150 82.813 0 150 67.188 150 150Z"/>
</svg>  // info.svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 128 300 300">
  <path d="M150 128c82.813 0 150 67.188 150 150 0 82.813-67.188 150-150 150C67.187 428 0 360.812 0 278c0-82.813 67.188-150 150-150Zm25 243.555v-37.11c0-3.515-2.734-6.445-6.055-6.445h-37.5c-3.515 0-6.445 2.93-6.445 6.445v37.11c0 3.515 2.93 6.445 6.445 6.445h37.5c3.32 0 6.055-2.93 6.055-6.445Zm-.39-67.188 3.515-121.289c0-1.367-.586-2.734-1.953-3.516-1.172-.976-2.93-1.562-4.688-1.562h-42.968c-1.758 0-3.516.586-4.688 1.563-1.367.78-1.953 2.148-1.953 3.515l3.32 121.29c0 2.734 2.93 4.882 6.64 4.882h36.134c3.515 0 6.445-2.148 6.64-4.883Z"/>
</svg>  // note.svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="300.5 134 300 300">
  <path d="M551.281 252.36c0-3.32-1.172-6.641-3.515-8.985l-17.774-17.578c-2.344-2.344-5.469-3.711-8.789-3.711-3.32 0-6.445 1.367-8.789 3.71l-79.687 79.493-44.141-44.14c-2.344-2.344-5.469-3.712-8.79-3.712-3.32 0-6.444 1.368-8.788 3.711l-17.774 17.579c-2.343 2.343-3.515 5.664-3.515 8.984 0 3.32 1.172 6.445 3.515 8.789l70.704 70.703c2.343 2.344 5.664 3.711 8.789 3.711 3.32 0 6.64-1.367 8.984-3.71l106.055-106.056c2.343-2.343 3.515-5.468 3.515-8.789ZM600.5 284c0 82.813-67.188 150-150 150-82.813 0-150-67.188-150-150 0-82.813 67.188-150 150-150 82.813 0 150 67.188 150 150Z"/>
</svg>  // tips.svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="126 76.5 300 300">
  <path d="M297.431 324.397v-34.255c0-3.245-2.344-5.95-5.358-5.95h-32.146c-3.014 0-5.358 2.705-5.358 5.95v34.255c0 3.245 2.344 5.95 5.358 5.95h32.146c3.014 0 5.358-2.705 5.358-5.95Zm-.335-67.428 3.014-82.753c0-1.081-.502-2.524-1.674-3.425-1.005-.902-2.512-1.983-4.019-1.983h-36.834c-1.507 0-3.014 1.081-4.019 1.983-1.172.901-1.674 2.704-1.674 3.786l2.846 82.392c0 2.344 2.512 4.146 5.693 4.146h30.975c3.013 0 5.525-1.803 5.692-4.146Zm-2.344-168.39L423.34 342.425c3.683 7.032 3.516 15.686-.335 22.717-3.85 7.031-10.883 11.358-18.417 11.358H147.413c-7.534 0-14.566-4.327-18.417-11.358-3.85-7.031-4.018-15.685-.335-22.716L257.248 88.578C260.93 81.188 268.13 76.5 276 76.5c7.87 0 15.069 4.688 18.752 12.08Z"/>
</svg>  // warning.svg
```

**示例** 删除'/'

``` shell
{/{< notice tip >}}
This is a tip.
{/{< /notice >}}
```

{{< notice tip >}}
This is a tip.
{{< /notice >}}

{{< notice warning >}}
This is a warning.
{{< /notice >}}

{{< notice info >}}
This is a info.
{{< /notice >}}

{{< notice note >}}
这是一个提示.
{{< /notice >}}


### 添加盘古之白
下载[pangu.min.js](https://cdn.bootcss.com/pangu/4.0.7/pangu.min.js)放置在assets/js/pangu.min.js

加载js文件 vim layouts/partials/extend_footer.html
``` html
<!-- 盘古之白中英文数字之间添加空格 --> 
{{- $highlight := resources.Get "js/pangu.min.js" }}
<script>
  (function (u, c) {
    var d = document,
      t = "script",
      o = d.createElement(t),
      s = d.getElementsByTagName(t)[0];
    o.src = u;
    if (c) {
      o.addEventListener("load", function (e) {
        c(e);
      });
    }
    s.parentNode.insertBefore(o, s);
  })("{{ $highlight.RelPermalink }}", function () {
    pangu.spacingPage();
  });
</script>
```

### 博客正文目录添加编号

> vim layouts/_default/single.html

``` html
{{- define "main" }}
<article class="post" {{- if .Param "autonumbering" }} autonumbering {{- end }}>
<!-- <article class="post-single"> -->
```

> vim assets/css/extended/blank.css

``` css
body {counter-reset: h2}
h2 {counter-reset: h3}
h3 {counter-reset: h4}
h4 {counter-reset: h5}

article[autonumbering] h2:before {counter-increment: h2; content: counter(h2) ". "}
article[autonumbering] h3:before {counter-increment: h3; content: counter(h2) "." counter(h3) " "}
article[autonumbering] h4:before {counter-increment: h4; content: counter(h2) "." counter(h3) "." counter(h4) " "}

article[autonumbering] .toc ul { counter-reset: item }
article[autonumbering] .toc li a:before { content: counters(item, ".") " "; counter-increment: item }
```

在头部添加配置 `autonumbering: true`
