# Debian Cloud Image Builder (for Proxmox VE)

使用 [CircleCI](https://circleci.com/) 自动构建适用于 Proxmox VE 的定制化 Debian 12 Cloud 镜像。

- 基于 [Debian official genericcloud image](https://cdimage.debian.org/images/cloud/)
- 安装常用包：`qemu-guest-agent`, `curl`, `wget`, `vim`, `htop`, `dnsutils` 等
- 自动清除 `/etc/machine-id`、日志和 apt 缓存
- 自动压缩为 `.qcow2`，上传至 GitHub Release

## 用法（在 PVE 上导入）

```bash
wget https://github.com/<你的用户名>/<repo>/releases/download/<版本>/debian-12-custom.qcow2
