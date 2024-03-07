---
title: kubernetes支持swap的Linux发行版最小安装对比
slug: >-
  kubernetes-supports-the-minimum-installation-comparison-of-the-linux-distribution-version-of-swan-kr6g8
url: >-
  /post/kubernetes-supports-the-minimum-installation-comparison-of-the-linux-distribution-version-of-swan-kr6g8.html
date: '2024-03-07 05:09:01'
lastmod: '2024-03-07 09:04:43'
toc: true
tags:
  - 运维
  - Linux
  - kubernetes
categories:
  - 技术
keywords: 运维,Linux,kubernetes
isCJKLanguage: true
---

# kubernetes支持swap的Linux发行版最小安装对比

## 结论：（最小安装以`/`​占用排名）

1. Fedora39（1.2G）
2. Debian-12.5.0（1.3G）
3. Arch Linux 2024.03.01（2.1G）
4. Ubuntu server 22.04 LTS（4.5G）

　　因此，Fedora和Debian选择自己熟悉的来吧

### Linux 发行版 cgroup v2 支持

　　有关使用 cgroup v2 的 Linux 发行版的列表， 请参阅 [cgroup v2 文档](https://github.com/opencontainers/runc/blob/main/docs/cgroup-v2.md)。

　　注：下面系统都按x64安装

* Container-Optimized OS（从 M97 开始）

  * 注：此Linux版本为云端专用版本，不作比较
* Ubuntu（从 21.10 开始，推荐 22.04+）

  * Ubuntu server 22.04 LTS 最小化安装占用4.5G

    ```bash
    szh@szh:~$ df -h
    Filesystem                         Size  Used Avail Use% Mounted on
    tmpfs                              197M  792K  196M   1% /run
    /dev/mapper/ubuntu--vg-ubuntu--lv   11G  4.5G  5.7G  45% /
    tmpfs                              982M     0  982M   0% /dev/shm
    tmpfs                              5.0M     0  5.0M   0% /run/lock
    /dev/nvme0n1p2                     2.0G  130M  1.7G   8% /boot
    /dev/nvme0n1p1                     1.1G  6.1M  1.1G   1% /boot/efi
    tmpfs                              197M  4.0K  197M   1% /run/user/1000
    ```
* Debian GNU/Linux（从 Debian 11 Bullseye 开始）

  * Debian-12.5.0只安装ssh和标准工具包`/`​占用为1.3G

    ```bash
    root@k8sNode1debian:~# df -h
    Filesystem                           Size  Used Avail Use% Mounted on
    udev                                 961M     0  961M   0% /dev
    tmpfs                                197M  696K  197M   1% /run
    /dev/mapper/k8sNode1debian--vg-root   15G  1.3G   13G   9% /
    tmpfs                                984M     0  984M   0% /dev/shm
    tmpfs                                5.0M     0  5.0M   0% /run/lock
    /dev/nvme0n1p1                       455M   58M  372M  14% /boot
    tmpfs                                197M     0  197M   0% /run/user/0
    ```
* Fedora（从 31 开始）

  * Fedora39 server 最小安装`/`​占用为1.2G

    ```bash
    [root@fedora ~]# df -h
    文件系统                 大小  已用  可用 已用% 挂载点
    /dev/mapper/fedora-root  6.4G  1.2G  5.2G   19% /
    devtmpfs                 4.0M     0  4.0M    0% /dev
    tmpfs                    982M     0  982M    0% /dev/shm
    efivarfs                 256K   48K  204K   19% /sys/firmware/efi/efivars
    tmpfs                    393M  728K  392M    1% /run
    tmpfs                    982M     0  982M    0% /tmp
    /dev/nvme0n1p2           960M  211M  750M   22% /boot
    /dev/nvme0n1p1           599M  7.5M  592M    2% /boot/efi
    tmpfs                    197M  4.0K  197M    1% /run/user/0
    ```
* Arch Linux（从 2021 年 4 月开始）

  * Arch Linux 2024.03.01版本，使用archinstall一键最小化安装`/`​占用为2.1G

    ```bash
    [root@archlinux ~]# df -h
    Filesystem      Size  Used Avail Use% Mounted on
    dev             978M     0  978M   0% /dev
    run             986M  688K  985M   1% /run
    efivarfs        256K   46K  206K  19% /sys/firmware/efi/efivars
    /dev/nvme0n1p2  7.5G  2.1G  5.1G  29% /
    tmpfs           986M     0  986M   0% /dev/shm
    tmpfs           986M     0  986M   0% /tmp
    /dev/nvme0n1p2  7.5G  2.1G  5.1G  29% /.snapshots
    /dev/nvme0n1p2  7.5G  2.1G  5.1G  29% /var/cache/pacman/pkg
    /dev/nvme0n1p2  7.5G  2.1G  5.1G  29% /home
    /dev/nvme0n1p2  7.5G  2.1G  5.1G  29% /var/log
    /dev/nvme0n1p1  511M   61M  451M  12% /boot
    tmpfs           198M  4.0K  198M   1% /run/user/0
    ```
* RHEL 和类似 RHEL 的发行版（从 9 开始）【本人没有企业授权测不了】
