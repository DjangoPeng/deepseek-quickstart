# Ubuntu 安装 Docker 和 Docker Compose 指南

## 1. 安装 Docker

### 更新软件包索引

```bash
sudo apt update
```

### 安装 Docker

```bash
sudo apt install docker.io -y
```

### 启动 Docker 服务并设置开机自启

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### 将当前用户加入 docker 组（避免每次使用 sudo）

```bash
sudo usermod -aG docker $USER
newgrp docker  # 立即生效，或重新登录终端
```

### 验证 Docker 安装

```bash
docker --version
# 预期输出示例: Docker version 24.0.7, build afdd53b
```

## 2. 安装 Docker Compose (方法一：官方二进制)

### 获取最新版本号

```bash
COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d '"' -f 4)
```

### 下载并安装 Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/local/bin/docker-compose
```

### 授予执行权限

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

### 验证安装

```bash
docker-compose --version
# 预期输出示例: Docker Compose version v2.26.1
```

## 3. 测试 Docker Compose

### 创建示例项目

```bash
mkdir docker-compose-demo && cd docker-compose-demo
```

### 创建 docker-compose.yml 文件

```yaml
version: '3'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
```

### 启动服务

```bash
docker-compose up -d
```

### 验证服务

```bash
curl localhost:8080  # 应返回 Nginx 欢迎页面
```

## 4. 常用命令

| 命令 | 说明 |
|------|------|
| `docker-compose up -d` | 启动服务（后台模式） |
| `docker-compose down` | 停止并移除容器 |
| `docker-compose ps` | 查看运行中的服务 |
| `docker-compose logs` | 查看服务日志 |

> **注意**：  
> - 如果遇到权限问题，请确认用户已加入 docker 组  
> - 国内用户可考虑使用镜像源加速下载（如阿里云镜像）  
> - Docker Compose v2+ 需要 Docker 20.10+ 版本支持
