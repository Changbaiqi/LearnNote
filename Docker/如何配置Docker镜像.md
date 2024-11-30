---
title: 如何配置Docker镜像
date: 2024-07-02 10:35:00
author: 长白崎
categories:
  - "Docker"
tags:
  - "Docker"
---



# 如何配置Docker镜像

---



## Dockerd 代理

**在执行docker pull时，是由守护进程dockerd来执行。因此，代理需要配在dockerd的环境中。而这个环境，则是受systemd所管控，因此实际是systemd的配置。**

1、创建dockerd相关的systemd目录，这个目录下的配置将覆盖dockerd的默认配置

`$ sudo mkdir -p /etc/systemd/system/docker.service.d`

新建配置文件`/etc/systemd/system/docker.service.d/proxy.conf`

```shell
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo touch /etc/systemd/system/docker.service.d/proxy.conf
```

在这个proxy.conf文件（可以是任意*.conf的形式）中，添加以下内容：

```shell
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:8080/"
Environment="HTTPS_PROXY=http://proxy.example.com:8080/"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

其中，[http://proxy.example.com:8080](https://link.zhihu.com/?target=http%3A//proxy.example.com%3A8080)要换成可用的免密代理。通常使用 cntlm 在本机自建免密代理，去对接公司的代理。可参考《Linux下安装配置Cntlm 代理》。

## Container 代理

在容器运行阶段，如果需要代理上网，则需要配置 ~/.docker/config.json。以下配置，只在Docker 17.07及以上版本生效。

```json
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://proxy.example.com:8080",
     "httpsProxy": "http://proxy.example.com:8080",
     "noProxy": "localhost,127.0.0.1,.example.com"
   }
 }
}
```

这个是用户级的配置，除了 proxies，docker login 等相关信息也会在其中。而且还可以配置信息展示的格式、插件参数等。

此外，容器的网络代理，也可以直接在其运行时通过 -e 注入 http_proxy 等环境变量。这两种方法分别适合不同场景。config.json 非常方便，默认在所有配置修改后启动的容器生效，适合个人开发环境。在CI/CD的自动构建环境、或者实际上线运行的环境中，这种方法就不太合适，用 -e 注入这种显式配置会更好，减轻对构建、部署环境的依赖。当然，在这些环境中，最好用良好的设计避免配置代理上网。

## Docker Build 代理

虽然 docker build 的本质，也是启动一个容器，但是环境会略有不同，用户级配置无效。在构建时，需要注入 http_proxy 等参数。

```shell
docker build . \
    --build-arg "HTTP_PROXY=http://proxy.example.com:8080/" \
    --build-arg "HTTPS_PROXY=http://proxy.example.com:8080/" \
    --build-arg "NO_PROXY=localhost,127.0.0.1,.example.com" \
    -t your/image:tag
```

**注意**：无论是 docker run 还是 docker build，默认是网络隔绝的。如果代理使用的是 localhost:3128 这类，则会无效。这类仅限本地的代理，必须加上 --network host 才能正常使用。而一般则需要配置代理的外部IP，而且代理本身要开启 Gateway 模式。



## 重启生效

代理配置完成后，reboot 重启当然可以生效，但不重启也行。

docker build 代理是在执行前设置的，所以修改后，下次执行立即生效。Container 代理的修改也是立即生效的，但是只针对以后启动的 Container，对已经启动的 Container 无效。

dockerd 代理的修改比较特殊，它实际上是改 systemd 的配置，因此需要重载 systemd 并重启 dockerd 才能生效。

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

