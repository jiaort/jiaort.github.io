---
author: ["Runtao Jiao"]
title: "安装Harbor"
date: "2025-11-18"
description: "通过docker-compose部署Harbor"
tags: ["harbor", "环境搭建"]
categories: ["环境搭建"]
autonumbering: true
cover:
  image: "/images/construction/harbor.jpeg"
  alt: "Harbor封面"
  relative: true
---
# Harbor 安装实践

Harbor 是一个用于存储和分发 Docker 镜像的企业级 Registry 服务，支持安全、身份认证、访问控制和审计日志等丰富功能。本文将介绍如何在 Linux 服务器上安装和配置 Harbor。

---

## 前置环境

在安装 Harbor 之前，请确保满足以下要求：

- 操作系统：CentOS 7+/Ubuntu 16.04+/其他主流 Linux 发行版
- 已安装 `docker`（建议 19.03 及以上版本）
- 已安装 `docker-compose`（建议 1.29.0 及以上版本）
- 至少 2C4G 服务器配置
- 可以访问外网（用于下载相关依赖包）

### 检查 Docker 和 Docker Compose

```bash
docker -v
docker-compose -v
```

如果未安装，请先安装 Docker 和 Docker Compose。可以参考[官方文档](https://docs.docker.com/engine/install/)。

---

## 下载 Harbor 安装包

前往 [Harbor 官方发布页](https://github.com/goharbor/harbor/releases) 下载最新离线安装包，例如：

```bash
wget https://github.com/goharbor/harbor/releases/download/v2.8.2/harbor-offline-installer-v2.8.2.tgz
```

解压安装包：

```bash
tar -zxvf harbor-offline-installer-*.tgz
cd harbor
```

---

## 配置 Harbor

复制默认配置文件并根据需求修改：

```bash
cp harbor.yml.tmpl harbor.yml
vim harbor.yml
```

常见修改项：

- `hostname`: 设置为你服务器的 IP 或域名
- `harbor_admin_password`: 初始管理员密码
- 如需使用 https，需配置证书相关内容

---

## 安装 Harbor

初始化 Harbor：

```bash
./prepare
```

启动 Harbor：

```bash
sudo ./install.sh
```

启动成功后，可以使用以下命令查看相关容器状态：

```bash
docker-compose ps
```

---

## 访问 Harbor 控制台

在浏览器访问 `http://你的服务器IP或域名`，使用上面配置的账号密码登录即可。

---

## 推送和拉取镜像示例

### 登录 Harbor

```bash
docker login 你的IP或域名
```

输入账号密码后登录成功。

### 推送镜像

```bash
# 给本地镜像打 tag
docker tag nginx:latest 你的IP或域名/library/nginx:latest

# 推送镜像到 Harbor
docker push 你的IP或域名/library/nginx:latest
```

---

## 常见问题

- 如启动报错，可查看日志 `docker-compose logs`
- 403 错误，多数为配置或鉴权问题，核查 `harbor.yml`
- Harbor 支持 https 配置，生产环境建议开启

---

## 参考链接

- [Harbor 官方文档](https://goharbor.io/docs/)
- [GitHub Releases](https://github.com/goharbor/harbor/releases)

如有疑问欢迎留言交流！
