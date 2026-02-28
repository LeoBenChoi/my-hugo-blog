# kali httpx 和 python 冲突问题解决

## 问题原因

今天（2025/11/20）使用kali的时候发现httpx无法正常使用了，排查了一会发现是python-httpx和go的httpx冲突了，导致进行网站探活的go的httpx无法正常使用……

## 解决方案

卸载python-httpx 使用go安装

**以下内容全部使用root账号进行**

1. 卸载python-httpx

```bash
# 检查 httpx 是否通过 apt 安装
dpkg -l | grep python3-httpx

# 卸载系统包
sudo apt remove python3-httpx -y

# 清理残留
sudo apt autoremove -y
```

2. 使用 `go install`（最新版本）

> 这里使用过apt install httpx-toolkit -y，但是httpx还是不能用

```bash
# 安装 Go（如果未安装）
sudo apt install golang -y

# 安装 httpx
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
```

这时 `~/go/bin/` 下面应该有一个httpx

```bash
# ls -liah ~/go/bin/httpx
7360654 -rwxr-xr-x 1 root root 61M Nov 20 21:16 /root/go/bin/httpx
```

3. 将二进制文件httpx移动到 `/usr/bin/` 下，httpx正常使用

> 也可以将 `~/go/bin/` 添加到PATH变量

```bash
cp ~/go/bin/httpx /usr/bin/
```

验证：

```
# httpx -version

    __    __  __       _  __
   / /_  / /_/ /_____ | |/ /
  / __ \/ __/ __/ __ \|   /
 / / / / /_/ /_/ /_/ /   |
/_/ /_/\__/\__/ .___/_/|_|
             /_/

                projectdiscovery.io

[INF] Current Version: v1.7.2
```