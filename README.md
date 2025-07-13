# 🇩🇪 Debian Cloud Image Builder for Proxmox VE

本项目使用 [CircleCI](https://circleci.com/) 自动构建适用于 Proxmox VE 的自定义 [Debian 12 Cloud Image](https://cdimage.debian.org/images/cloud/bookworm/latest/)，并自动上传至 GitHub Release。

This project uses [CircleCI](https://circleci.com/) to automatically build a customized [Debian 12 Cloud Image](https://cdimage.debian.org/images/cloud/) suitable for Proxmox VE, and publishes it to GitHub Releases.

---

## ✅ 镜像特性 | Image Features

- 基于官方 `debian-12-genericcloud-amd64.qcow2`
- 预装实用软件：`qemu-guest-agent`, `curl`, `vim`, `htop`, `dnsutils` 等
- 支持 Cloud-init、DHCP 获取 IP
- 可直接导入至 Proxmox VE 作为模板

- Based on official `debian-12-genericcloud-amd64.qcow2`
- Includes useful packages like `qemu-guest-agent`, `curl`, `vim`, `htop`, `dnsutils`, etc.
- Cloud-init ready, DHCP enabled
- Easily importable as a Proxmox VE template

---

## 📥 下载最新镜像 | Download Latest Image

```bash
wget https://github.com/xixi-zhao/debian-cloud-build/releases/download/latest/debian-12-custom.qcow2
```

（请确保你在 GitHub Release 页面中打了一个 tag 叫 `latest`）

*(Make sure to maintain a tag named `latest` on GitHub Releases.)*

---

## 💻 导入到 Proxmox VE | Import into Proxmox VE

```bash
# 创建 VM（虚拟机）
qm create 9000 --name debian-template --memory 1024 --net0 virtio,bridge=vmbr0 \
  --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-9000-disk-0 --boot order=scsi0 \
  --ide2 local-lvm:cloudinit --serial0 socket --vga serial0 --agent enabled=1

# 上传磁盘
qm importdisk 9000 debian-12-custom.qcow2 local-lvm --format qcow2
qm set 9000 --scsi0 local-lvm:vm-9000-disk-0

# 转为模板
qm template 9000
```

---

## 🔁 自动构建流程 | Build Workflow

每次打 tag（如 `v2.13`）时，将：

- 自动构建定制镜像
- 上传 `.qcow2` 至对应 GitHub Release
- 可选打 `latest` tag 用于统一下载链接

Every time a version tag (e.g., `v2.13`) is pushed:

- A custom image is built
- The `.qcow2` is uploaded to GitHub Releases
- Optionally update the `latest` tag for stable URLs

---

## 🧩 修改建议 | Customization

- 修改 `.circleci/config.yml` 可添加或删除预装软件
- 可适配 OpenStack/KVM/VirtualBox 等平台（需转换）

To change installed packages, edit `.circleci/config.yml`. You can also adapt the image for other platforms (OpenStack, VirtualBox, etc.) if needed.

---

## 🤝 参考资源 | References

- [Sukka's Blog - 自定义 Cloud 镜像](https://blog.skk.moe/)
- [Debian Cloud 官方镜像](https://cdimage.debian.org/images/cloud/)

---

## 🛠️ License | 许可证

本项目使用 MIT 许可证（见 [LICENSE](LICENSE) 文件）。  
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
