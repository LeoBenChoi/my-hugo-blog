---
title: Kali 当前用户目录中英文修改
date: 2025-12-11T20:13:40+08:00
draft: true
toc: true
tags:
  - Kali
  - Linux
  - xdg-user-dirs
  - i18n
categories:
  - Linux
  - 解决方案
---
# Kali 当前用户目录中英文修改

>本篇文章由ai生成

Kali Linux 默认会为中文用户创建「桌面 / 下载 / 文档 …」等中文目录。  
如果只想把 **当前用户** 的目录名换成英文，而 **不动系统级语言**，可以用 `xdg-user-dirs` 快速完成。

## 方式1 一键脚本（推荐）

把下面内容保存为 `switch-home-dirs-en.sh`，`chmod +x` 后执行即可。

```bash
#!/usr/bin/env bash
set -e

# 1. 让 xdg 以后都使用英文路径
xdg-user-dirs-update --set DESKTOP       "$HOME/Desktop"
xdg-user-dirs-update --set DOWNLOAD      "$HOME/Downloads"
xdg-user-dirs-update --set TEMPLATES     "$HOME/Templates"
xdg-user-dirs-update --set PUBLICSHARE   "$HOME/Public"
xdg-user-dirs-update --set DOCUMENTS     "$HOME/Documents"
xdg-user-dirs-update --set MUSIC         "$HOME/Music"
xdg-user-dirs-update --set PICTURES      "$HOME/Pictures"
xdg-user-dirs-update --set VIDEOS        "$HOME/Videos"

# 2. 如果已存在中文目录，则重命名
for src in 桌面 下载 模板 公共 文档 音乐 图片 视频; do
    [ -d "$HOME/$src" ] && mv "$HOME/$src" "$HOME/$(xdg-user-dir "$(tr '[:lower:]' '[:upper:]' <<< "$src")")"
done

echo "✅ 完成！注销或重启会话后生效。"
```

## 方式2 手动分步执行

终端逐条复制即可：

```bash
# 设置英文路径记录
xdg-user-dirs-update --set DESKTOP       ~/Desktop
xdg-user-dirs-update --set DOWNLOAD      ~/Downloads
xdg-user-dirs-update --set TEMPLATES     ~/Templates
xdg-user-dirs-update --set PUBLICSHARE   ~/Public
xdg-user-dirs-update --set DOCUMENTS     ~/Documents
xdg-user-dirs-update --set MUSIC         ~/Music
xdg-user-dirs-update --set PICTURES      ~/Pictures
xdg-user-dirs-update --set VIDEOS        ~/Videos

# 重命名现有中文目录
mv ~/桌面   ~/Desktop   2>/dev/null
mv ~/下载   ~/Downloads 2>/dev/null
mv ~/模板   ~/Templates 2>/dev/null
mv ~/公共   ~/Public    2>/dev/null
mv ~/文档   ~/Documents 2>/dev/null
mv ~/音乐   ~/Music     2>/dev/null
mv ~/图片   ~/Pictures  2>/dev/null
mv ~/视频   ~/Videos    2>/dev/null
```

## 3. 验证

注销或重启图形会话 → 打开文件管理器 → 家目录应全部显示英文：

```
Desktop  Downloads  Templates  Public  Documents  Music  Pictures  Videos
```

## 4. 恢复中文目录

改回中文，执行：

```bash
export LANG=zh_CN.UTF-8
xdg-user-dirs-update
```

随后手动把目录名字改回中文即可。

## 5. 注意事项

- **仅影响当前用户**，系统语言、登录界面等保持不变。  
- 依赖 `xdg-user-dirs`，Kali 默认已安装；若缺失 `sudo apt install xdg-user-dirs`。  
- 对基于 XFCE/GNOME/KDE 的 Kali 均有效。

---

完。
