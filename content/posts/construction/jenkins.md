---
author: ["Runtao Jiao"]
title: "安装Jenkins"
date: "2025-11-18"
description: "通过docker-compose部署Jenkins"
tags: ["jenkins", "环境搭建"]
categories: ["环境搭建"]
autonumbering: true
cover:
  image: "/images/construction/jenkins.png"
  alt: "Jenkins封面"
  relative: true
---
# Jenkins自动化部署实践

Jenkins 是开源的自动化服务平台，用于持续集成与交付（CI/CD）。本教程将指导你通过 `docker-compose` 快速部署 Jenkins.

---

## 环境准备

- 操作系统：推荐 Linux (如 Ubuntu 18.04+)
- 已安装 [Docker](https://docs.docker.com/engine/install/) 与 [Docker Compose](https://docs.docker.com/compose/install/)
- 服务器开放 8080（HTTP）端口

---

## 编写 Dockerfile 安装相关插件
可以通过 plugins.txt 在启动时安装插件，示例如下：
```shell
# plugins.txt
blueocean
docker-workflow
git
ssh-agent
pipeline-utility-steps
configuration-as-code
```
Dockerfile 配置如下：
```shell
FROM jenkins/jenkins:2.528.1-jdk21  # 国内源：swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/jenkins/jenkins:2.528.1-lts-jdk21


USER root

RUN apt-get update && \
    apt-get install -y lsb-release ca-certificates curl

RUN install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc && \
    chmod a+r /etc/apt/keyrings/docker.asc

RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
    https://download.docker.com/linux/debian $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" \
    | tee /etc/apt/sources.list.d/docker.list > /dev/null

RUN apt-get update && \
    apt-get install -y docker-ce-cli && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# USER jenkins

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt
```

---

## docker-compose.yml 配置

在你的工作目录新建 docker-compose.yml ，内容如下：

```yaml
services:
  jenkins:
    image: my-jenkins:lts-gitlab
    build:
      context: .
    privileged: true
    volumes:
      - jenkins-data:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock  # 映射本地docker配置，方便在容器内部构建镜像
      - /home:/home
    ports:
      - 8080:8080
    environment:
      - TZ=Asia/Shanghai
    extra_hosts:
      - "git.huaxin-hitec.com:101.201.100.217"
    logging:
      driver: json-file
      options:
        max-file: '10'
        max-size: 10m
volumes:
  jenkins-data:
    external: false
```
---

## 启动 Jenkins

在 `docker-compose.yml` 所在目录执行：

```shell
docker-compose up -d
```

首次启动会自动初始化 Jenkins，访问方式如下：

- **HTTP**: [http://服务器IP:8080](http://服务器IP:8080)

进入容器获取初始管理员密码：

```shell
cat /var/jenkins_home/secrets/initialAdminPassword
```

---

## Docker Compose 管理常用命令

- 启动服务: `docker-compose up -d`
- 查看日志: `docker-compose logs -f`
- 停止服务: `docker-compose down`

---

# Jenkins 配置

## 添加需要的 Jenkins 插件
Jenkins 默认安装的插件比较少，需要添加需要的插件。

## 添加需要的 Jenkins 凭据
系统管理 / 凭据 / 系统 / 全局凭据 (unrestricted) / Add Credentials
如： gitlab | Harbor 通过 access token 登录

## Jenkins 流水线配置示例
```pipeline
pipeline {
    agent any

    environment {
        GIT_REPO   = 'https://git.code.com/xxx.git'
        GIT_BRANCH = 'main'
        GIT_CRED   = 'gitlab-https-token'
        REGISTRY   = 'hub.docker.com'
        REG_CRED   = 'harbor-token'
        IMAGE_NAME = 'dev/demo'
    }

    stages {
        stage('Checkout / Update Code') {
            steps {
                // 设置安全目录，防止 dubious ownership 错误
                sh "git config --global --add safe.directory ${env.WORKSPACE}"

                // 使用 Jenkins HTTPS 凭据
                withCredentials([usernamePassword(credentialsId: "${GIT_CRED}", usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    script {
                        // 构造带凭据的 HTTPS 地址
                        def authRepo = GIT_REPO.replace('https://', "https://${GIT_USER}:${GIT_PASS}@")

                        if (fileExists("${env.WORKSPACE}/.git")) {
                            echo "📦 项目已存在，开始更新..."
                            sh """
                                git remote set-url origin ${authRepo}
                                git fetch origin ${GIT_BRANCH}
                                git reset --hard origin/${GIT_BRANCH}
                                git clean -fd
                            """
                        } else {
                            echo "🆕 项目不存在，开始首次克隆..."
                            sh "git clone -b ${GIT_BRANCH} ${authRepo} ${env.WORKSPACE}"
                        }

                        // 获取当前 commit ID
                        env.COMMIT_ID = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                        echo "🧩 当前提交 Commit ID: ${env.COMMIT_ID}"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🔧 构建 Docker 镜像: ${REGISTRY}/${IMAGE_NAME}:${env.COMMIT_ID}"
                    sh "docker build -t ${REGISTRY}/${IMAGE_NAME}:${env.COMMIT_ID} ${env.WORKSPACE}"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${REG_CRED}", usernameVariable: 'REG_USER', passwordVariable: 'REG_PASS')]) {
                    script {
                        echo "🚀 登录 Docker 仓库 ${REGISTRY}"
                        sh """
                            docker login -u ${REG_USER} -p ${REG_PASS} ${REGISTRY}
                            docker push ${REGISTRY}/${IMAGE_NAME}:${env.COMMIT_ID}
                        """
                    }
                }
            }
        }

        stage('Clean Environment') {
            steps {
                script {
                    echo "🧹 清理工作环境与临时 Docker 资源"
                    // 清理 workspace
                    // deleteDir()
                    // 清理构建产生的 Docker 镜像和未使用资源
                    sh """
                        docker rmi -f ${REGISTRY}/${IMAGE_NAME}:${env.COMMIT_ID} || true
                        docker system prune -af --volumes
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ 构建成功，镜像标签：${REGISTRY}/${IMAGE_NAME}:${env.COMMIT_ID}"
        }
        failure {
            echo "❌ Pipeline 构建失败"
        }
    }
}
```

---

# 总结

使用 docker-compose 可实现Jenkins的快速安装与迁移，并结合证书配置保障安全访问。适合个人与团队的高效自动化部署。欢迎留言交流部署遇到的问题！
