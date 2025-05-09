# Runtao's Blog

这是一个基于 [Hugo](https://gohugo.io/) 构建的个人博客项目，主题为 `PaperMod`，用于分享生活、阅读和学习内容。

## 项目简介

- **站点地址**：<https://jiaort.github.io>
- **作者**：Runtao Jiao
- **描述**：一个分享生活、阅读、学习的个人博客。
- **默认语言**：中文（zh-cn）

## 主要特性

- 支持 Emoji、代码高亮、目录、字数统计、阅读时间、访问计数
- 支持评论、社交分享、Busuanzi 流量统计
- 多主题切换（自动/明亮/暗色）
- 个人简介区块（Profile Mode）
- 丰富的菜单导航与分类标签体系
- 支持 RSS、JSON 输出
- 支持 Fancybox 图片查看器
- 支持 Fuse.js 本地搜索

## 目录结构说明

- `content/`：博客内容（文章、分类、标签、关于、友链等）
- `themes/`：主题文件夹（已集成 `PaperMod`）
- `static/`：静态资源（如图片、favicon 等）
- `assets/`：自定义 JS/CSS 资源
- `archetypes/`：内容模板（如 `default.md` 用于新建文章）
- `layouts/`：自定义页面布局
- `data/`、`i18n/`：数据与多语言支持
- `public/`：Hugo 生成的静态站点输出目录（自动生成）

## 快速开始

1. **安装 Hugo**
   ```bash
   brew install hugo
   # 或参考 https://gohugo.io/getting-started/installing/
   ```

2. **克隆本项目**
   ```bash
   git clone https://github.com/jiaort/your-repo.git
   cd your-repo
   ```

3. **初始化/更新主题子模块**
   ```bash
   git submodule update --init --recursive
   ```

4. **本地预览**
   ```bash
   hugo server -D
   # 访问 http://localhost:1313
   ```

5. **发布到 GitHub Pages**
   ```bash
   hugo
   # 将 public/ 目录内容推送到你的 GitHub Pages 仓库
   ```

## 内容创作

- 新建文章：
  ```bash
  hugo new posts/你的文章标题.md
  ```
- 编辑 `content/` 下的 Markdown 文件，支持 Front Matter 配置（如标题、日期、草稿等）。

## 主题与自定义

- 主题配置详见 `hugo.yaml`，可根据需要调整菜单、社交信息、外观等。
- 静态资源如头像、favicon 请放在 `static/images/` 和 `static/favicons/` 下。

## 许可证

本项目采用 MIT License，详见 LICENSE 文件。

---

如需进一步个性化或有其他问题，欢迎联系作者或提交 Issue。
