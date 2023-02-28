# Docker安装

[TOC]

# [安装](https://docs.docker.com/engine/install/ubuntu/)

## 卸载旧版本

旧版本的 Docker 称为 `docker` 或者 `docker-engine`，使用以下命令卸载旧版本：

```
sudo apt-get remove docker docker-engine docker.io containerd runc
```

鉴于国内网络问题，强烈建议使用国内源，官方源请在注释中查看。

为了确认所下载软件包的合法性，需要添加软件源的 `GPG` 密钥。

```
sudo mkdir -m 0755 -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# 官方源
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

然后，我们需要向 `sources.list` 中添加 Docker 软件源

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


# 官方
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

> 以上命令会添加稳定版本的 Docker APT 镜像源，如果需要测试版本的 Docker 请将 stable 改为 test。

## 安装 Docker

更新 apt 软件包缓存，并安装 `docker-ce`：

```
sudo apt-get update
```

报错
```
Receiving a GPG error when running apt-get update?

sudo chmod a+r /etc/apt/keyrings/docker.gpg
sudo apt-get update
```

安装docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 启动Docker

```
# 开机启动
sudo systemctl enable docker
# 启动服务
sudo systemctl start docker
```

## 建立 docker 用户组

默认情况下，`docker` 命令会使用 [Unix socket](https://en.wikipedia.org/wiki/Unix_domain_socket) 与 Docker 引擎通讯。而只有 `root` 用户和 `docker` 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 `root` 用户。因此，更好地做法是将需要使用 `docker` 的用户加入 `docker` 用户组。

建立 `docker` 组：

```
sudo groupadd docker
```

将当前用户加入 `docker` 组：

```
sudo usermod -aG docker $USER
```

***退出当前终端并重新登录***，进行如下测试。

## 测试 Docker 是否安装正确

```
docker run --rm hello-world
```

## 镜像加速器

如果在使用过程中发现拉取 Docker 镜像十分缓慢，可以配置 Docker [国内镜像加速](https://yeasy.gitbook.io/docker_practice/install/mirror)。

# 参考文档

- [Docker一从入门到实践](https://yeasy.gitbook.io/docker_practice/)

- 微信读书：每天5分钟玩转Docker容器技术

- [阿里云 Docker CE镜像](https://developer.aliyun.com/mirror/docker-ce/)

- [镜像加速器](https://yeasy.gitbook.io/docker_practice/install/mirror)
