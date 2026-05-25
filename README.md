<div align="center">

# 🌸 Lotus Linux

**A minimal, independent Linux distribution built from scratch.**  
**Một bản phân phối Linux tối giản, độc lập, xây dựng từ đầu.**

[![License: MIT](https://img.shields.io/badge/License-MIT-cyan.svg)](LICENSE)
![Status](https://img.shields.io/badge/status-Closed%20Beta-yellow)
![Arch](https://img.shields.io/badge/arch-x86__64-blue)
![Libc](https://img.shields.io/badge/libc-musl-green)
![Init](https://img.shields.io/badge/init-dinit-purple)

</div>

---

## 🇬🇧 English

### What is Lotus Linux?

Lotus Linux is a minimal, independent Linux distribution built entirely from scratch — no Debian, no Arch, no Ubuntu base. Just the kernel, musl libc, BusyBox, dinit, and a clean live environment.

Born on **May 24, 2026** — the founder's birthday.

### Features

- **Kernel:** Linux 6.18.10 (custom config)
- **libc:** musl 1.2.6 (static-preferred)
- **Init:** dinit
- **Shell:** bash 5.3 + BusyBox 1.37
- **Bootloader:** GRUB (BIOS + UEFI)
- **Live ISO:** ~65MB squashfs overlay
- **Package manager:** lpm (Lotus Package Manager)
- **Device manager:** mdevd
- **Network:** dhcpcd + wget (HTTPS via OpenSSL)
- **Filesystem tools:** e2fsprogs, util-linux, busybox fdisk

### Download

| File | Description |
|------|-------------|
| `lotus-linux-livecd-20260524.iso` | Bootable Live ISO |
| `lotus-linux-rootfs-20260524-stable.tar.xz` | Root filesystem tarball |

### Installation Guide

**1. Boot from Live ISO**

**2. Connect to network**
```bash
dhcpcd eth0          # wired
iwctl                # WiFi
```

**3. Partition your disk**
```bash
fdisk /dev/sda
```

**4. Format**
```bash
mkfs.ext4 /dev/sda2
mkfs.fat -F32 /dev/sda1   # EFI only
```

**5. Mount**
```bash
mount /dev/sda2 /mnt
```

**6. Download & extract rootfs**
```bash
wget https://github.com/draconmc1337/lotus-linux/releases/download/v0.1/lotus-linux-rootfs-20260524-stable.tar.xz
tar xJf lotus-linux-rootfs-20260524-stable.tar.xz -C /mnt
```

**7. Chroot**
```bash
lotus-chroot /mnt
```

**8. Configure**
```bash
ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime
echo "mylotus" > /etc/hostname
passwd root
```

**9. Install bootloader**
```bash
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

**10. Reboot**
```bash
exit
umount -R /mnt
reboot
```

### Package Manager

```bash
lpm sync              # sync package list
lpm install <pkg>     # install package
lpm remove <pkg>      # remove package
lpm upgrade           # upgrade all
lpm search <pkg>      # search
```

### Philosophy

> *Simple. Clean. Yours.*

Lotus Linux is built by one person, for people who want to understand their system — not just use it.

No bloat. No unnecessary dependencies. Just what you need.

---

## 🇻🇳 Tiếng Việt

### Lotus Linux là gì?

Lotus Linux là một bản phân phối Linux tối giản, độc lập — không dựa trên Debian, Arch hay Ubuntu. Chỉ có kernel, musl libc, BusyBox, dinit và một môi trường live sạch sẽ.

Ra đời ngày **24 tháng 5 năm 2026** — đúng ngày sinh nhật của người tạo ra nó:))

### Tính năng

- **Kernel:** Linux 6.18.10 (config tùy chỉnh)
- **libc:** musl 1.2.6 (ưu tiên static)
- **Init:** dinit
- **Shell:** bash 5.3 + BusyBox 1.37
- **Bootloader:** GRUB (BIOS + UEFI)
- **Live ISO:** ~65MB squashfs overlay
- **Quản lý gói:** lpm (Lotus Package Manager)
- **Quản lý thiết bị:** mdevd
- **Mạng:** dhcpcd + wget (HTTPS qua OpenSSL)
- **Công cụ filesystem:** e2fsprogs, util-linux, busybox fdisk

### Tải về

| File | Mô tả |
|------|-------|
| `lotus-linux-livecd-20260524.iso` | Live ISO có thể boot |
| `lotus-linux-rootfs-20260524-stable.tar.xz` | Tarball rootfs để cài đặt |

### Hướng dẫn cài đặt

**1. Boot từ Live ISO**

**2. Kết nối mạng**
```bash
dhcpcd eth0          # có dây
iwctl                # WiFi
```

**3. Phân vùng ổ đĩa**
```bash
fdisk /dev/sda
# Gợi ý:
# /dev/sda1  512MB  EFI System (nếu UEFI)
# /dev/sda2  20GB+  Linux filesystem (/)
# /dev/sda3  2GB    Linux swap (tùy chọn)
```

**4. Format**
```bash
mkfs.ext4 /dev/sda2
mkfs.fat -F32 /dev/sda1   # chỉ khi UEFI
mkswap /dev/sda3 && swapon /dev/sda3
```

**5. Mount**
```bash
mount /dev/sda2 /mnt
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi   # chỉ khi UEFI
```

**6. Tải và giải nén rootfs**
```bash
wget https://github.com/draconmc1337/lotus-linux/releases/download/v0.1/lotus-linux-rootfs-20260524-stable.tar.xz
tar xJf lotus-linux-rootfs-20260524-stable.tar.xz -C /mnt
```

**7. Chroot vào hệ thống**
```bash
lotus-chroot /mnt
```

**8. Cấu hình cơ bản**
```bash
# timezone
ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime
echo "Asia/Ho_Chi_Minh" > /etc/timezone

# hostname
echo "mylotus" > /etc/hostname

# fstab
cat > /etc/fstab << 'EOF'
/dev/sda2   /         ext4   defaults   0 1
/dev/sda1   /boot/efi vfat   defaults   0 2
/dev/sda3   none      swap   sw         0 0
EOF

# đặt mật khẩu root
passwd root
```

**9. Cài bootloader**
```bash
# BIOS
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

# UEFI
grub-install --target=x86_64-efi \
  --efi-directory=/boot/efi \
  --bootloader-id=lotus
grub-mkconfig -o /boot/grub/grub.cfg
```

**10. Reboot**
```bash
exit
umount -R /mnt
reboot
```

### Quản lý gói

```bash
lpm sync              # đồng bộ danh sách gói
lpm install <gói>     # cài gói
lpm remove <gói>      # gỡ gói
lpm upgrade           # nâng cấp tất cả
lpm search <gói>      # tìm kiếm
```

### Triết lý

> *Đơn giản. Sạch sẽ. Của bạn.*

Lotus Linux được xây dựng bởi một người, dành cho những người muốn hiểu hệ thống của mình — không chỉ đơn giản là sử dụng nó.

Không bloat. Không dependency không cần thiết. Chỉ những gì bạn cần.

---

## Building from source

```bash
git clone https://github.com/draconmc1337/lotus-linux
cd lotus-linux
# xem docs/building.md
```

## Contributing

Issues và Pull Requests luôn được chào đón!

---

<div align="center">

**Lotus Linux** — Made with ❤️ in Vietnam 🇻🇳

*Born: May 24, 2026*

</div>
