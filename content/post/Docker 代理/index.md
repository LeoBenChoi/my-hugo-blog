+++
date = '2025-11-19T23:08:02+08:00'
draft = false
title = 'Docker 代理'
image='index.png'
tags = [
  "代理",
]
categories = [
  "DevOps",
]
+++

# Docker 代理

1. 创建 `docker.service.d` 目录

``` bash
sudo mkdir /etc/systemd/system/docker.service.d/
```

2. 修改代理 

将 `proxy-server:port` 改为自己的代理地址和端口


```bash
sudo tee /etc/systemd/system/docker.service.d/proxy.conf << EOF 
[Service]
Environment="HTTP_PROXY=http://proxy-server:port"
Environment="HTTPS_PROXY=http://proxy-server:port"
Environment="NO_PROXY=localhost,127.0.0.1"
EOF
```

3. 重新加载 `systemd` 配置

```bash
sudo systemctl daemon-reload
```

4. 重启 `Docker` 服务

```bash
sudo systemctl restart docker
```

5. 验证（搜个ubuntu看看）

```
sudo docker search ubuntu
```

