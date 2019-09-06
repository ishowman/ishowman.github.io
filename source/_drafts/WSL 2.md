---
title: Develop on WSL2
date: 2019-08-08 21:01:15
category: develop-environment
tags: - WSL2
---

<h1>记录在 win10 wsl2 环境下的开发 </h1>

## 大纲
- wsl 2 带来了什么
- wsl2 的安装
- wsl2 的配置
- 在 wsl2 中开发的注意事项
  - webpack 的 hmr 不生效。原因是代码放在 windows
  下的盘，虽然在 vscode 中通过 /mnt 路径打开，但被认为是非 linux 系统下的文件。因此修改保存也不会自动刷新。
  - 解决方案[参考](https://www.reddit.com/r/bashonubuntuonwindows/comments/c48yej/wsl_2_react_not_reloading_with_file_changes/)。