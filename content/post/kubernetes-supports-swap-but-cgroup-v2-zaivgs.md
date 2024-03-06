---
title: kubernetes支持swap了，但是cgroup v2
slug: kubernetes-supports-swap-but-cgroup-v2-zaivgs
url: /post/kubernetes-supports-swap-but-cgroup-v2-zaivgs.html
date: '2024-03-06 20:13:53'
lastmod: '2024-03-06 20:27:37'
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

# kubernetes支持swap了，但是cgroup v2

## 结论：

　　Container-Optimized OS（从 M97 开始）、Ubuntu（从 21.10 开始，推荐 22.04+）、Debian GNU/Linux（从 Debian 11 Bullseye 开始）、Fedora（从 31 开始）、Arch Linux（从 2021 年 4 月开始）、RHEL 和类似 RHEL 的发行版（从 9 开始）支持使用swap但是要配置，未配置前先用`sudo swapoff -a`​暂时关闭。  
其他系统要手动在内核配置cgroup v2

### 来自官方文档：

　　交换分区的配置。kubelet 的默认行为是在节点上检测到交换内存时无法启动。 kubelet 自 v1.22 起已开始支持交换分区。自 v1.28 起，仅针对 cgroup v2 支持交换分区； kubelet 的 NodeSwap 特性门控处于 Beta 阶段，但默认被禁用。

* 如果 kubelet 未被正确配置使用交换分区，则你**必须**禁用交换分区。 例如，`sudo swapoff -a`​ 将暂时禁用交换分区。要使此更改在重启后保持不变，请确保在如 `/etc/fstab`​、`systemd.swap`​ 等配置文件中禁用交换分区，具体取决于你的系统如何配置。

### cgroup v2

　　 要检查你的发行版使用的是哪个 cgroup 版本，请在该节点上运行 `stat -fc %T /sys/fs/cgroup/`​ 命令：

```shell
stat -fc %T /sys/fs/cgroup/
```

　　对于 cgroup v2，输出为 `cgroup2fs`​。

　　对于 cgroup v1，输出为 `tmpfs`​。

　　cgroup v2 具有以下要求：

* 操作系统发行版启用 cgroup v2
* Linux 内核为 5.8 或更高版本
* 容器运行时支持 cgroup v2。例如：

  * [containerd](https://containerd.io/) v1.4 和更高版本
  * [cri-o](https://cri-o.io/) v1.20 和更高版本
* kubelet 和容器运行时被配置为使用 [systemd cgroup 驱动](https://kubernetes.io/zh-cn/docs/setup/production-environment/container-runtimes#systemd-cgroup-driver)

### Linux 发行版 cgroup v2 支持

　　有关使用 cgroup v2 的 Linux 发行版的列表， 请参阅 [cgroup v2 文档](https://github.com/opencontainers/runc/blob/main/docs/cgroup-v2.md)。

* Container-Optimized OS（从 M97 开始）
* Ubuntu（从 21.10 开始，推荐 22.04+）
* Debian GNU/Linux（从 Debian 11 Bullseye 开始）
* Fedora（从 31 开始）
* Arch Linux（从 2021 年 4 月开始）
* RHEL 和类似 RHEL 的发行版（从 9 开始）

　　要检查你的发行版是否使用 cgroup v2， 请参阅你的发行版文档或遵循[识别 Linux 节点上的 cgroup 版本](https://kubernetes.io/zh-cn/docs/concepts/architecture/cgroups/#check-cgroup-version)中的指示说明。

　　你还可以通过修改内核 cmdline 引导参数在你的 Linux 发行版上手动启用 cgroup v2。 如果你的发行版使用 GRUB，则应在 `/etc/default/grub`​ 下的 `GRUB_CMDLINE_LINUX`​ 中添加 `systemd.unified_cgroup_hierarchy=1`​， 然后执行 `sudo update-grub`​。不过，推荐的方法仍是使用一个默认已启用 cgroup v2 的发行版。
