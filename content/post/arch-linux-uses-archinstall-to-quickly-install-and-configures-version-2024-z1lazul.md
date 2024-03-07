---
title: Arch Linux使用archinstall快速安装配置2024版
slug: >-
  arch-linux-uses-archinstall-to-quickly-install-and-configures-version-2024-z1lazul
url: >-
  /post/arch-linux-uses-archinstall-to-quickly-install-and-configures-version-2024-z1lazul.html
date: '2024-03-07 07:53:14'
lastmod: '2024-03-07 10:16:56'
toc: true
tags:
  - 运维
  - Linux
  - Arch Linux
categories:
  - 技术
keywords: 运维,Linux,Arch Linux
isCJKLanguage: true
---

# Arch Linux使用archinstall快速安装配置2024版

　　参考：https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97

　　官方安装指南比较繁琐，有提供`archinstall`​但是并没有详细介绍对应配置项，本教程根据自身安装经历编写

## 安装前的准备 <span style="font-weight: bold;" class="bold">[</span>​[编辑](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&veaction=edit&section=1 "编辑章节：安装前的准备")  <span style="font-weight: bold;" class="bold">|</span>  [编辑源代码](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&action=edit&section=1 "编辑章节的源代码： 安装前的准备")]

### 获取安装映像 <span style="font-weight: bold;" class="bold">[</span>​[编辑](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&veaction=edit&section=2 "编辑章节：获取安装映像")  <span style="font-weight: bold;" class="bold">|</span>  [编辑源代码](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&action=edit&section=2 "编辑章节的源代码： 获取安装映像")]

　　打开[下载](https://archlinux.org/download/)页面，并根据需要的引导方式，获取 ISO 文件或 netboot 映像以及相应的 [GnuPG](https://wiki.archlinuxcn.org/wiki/GnuPG "GnuPG") 签名。

　　阿里源：https://mirrors.aliyun.com/archlinux/iso/latest/

　　页面如下：

```bash
Index of /archlinux/iso/latest/
File Name	File Size	Date
Parent directory/	-	-
arch/	-	2024-03-01 23:08
archlinux-2024.03.01-x86_64.iso	942.3 MB	2024-03-01 23:08
archlinux-2024.03.01-x86_64.iso.sig	141.0 B	2024-03-01 23:09
archlinux-2024.03.01-x86_64.iso.torrent	57.7 KB	2024-03-01 23:10
archlinux-bootstrap-2024.03.01-x86_64.tar.gz	182.5 MB	2024-03-01 23:09
archlinux-bootstrap-2024.03.01-x86_64.tar.gz.sig	141.0 B	2024-03-01 23:10
archlinux-bootstrap-x86_64.tar.gz	182.5 MB	2024-03-01 23:10
archlinux-bootstrap-x86_64.tar.gz.sig	141.0 B	2024-03-01 23:10
archlinux-x86_64.iso	942.3 MB	2024-03-01 23:10
archlinux-x86_64.iso.sig	141.0 B	2024-03-01 23:10
b2sums.txt	652.0 B	2024-03-01 23:10
sha256sums.txt	396.0 B	2024-03-01 23:10
```

　　点击archlinux-x86_64.iso下载

### （可选）验证签名 <span style="font-weight: bold;" class="bold">[</span>​[编辑](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&veaction=edit&section=3 "编辑章节：验证签名")  <span style="font-weight: bold;" class="bold">|</span>  [编辑源代码](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&action=edit&section=3 "编辑章节的源代码： 验证签名")]

　　<u>注：一般情况下载的都不会有问题，</u>​<u>不验证也可以</u>

　　建议使用前先验证所下载文件的签名，特别是从 <span style="font-weight: bold;" class="bold">HTTP 镜像源</span>下载的文件，因为 HTTP 连接一般来说容易遭到拦截而[提供恶意镜像](http://www2.cs.arizona.edu/stork/packagemanagersecurity/attacks-on-package-managers.html)。

　　在一台已经安装 [GnuPG](https://wiki.archlinuxcn.org/wiki/GnuPG "GnuPG") 的系统上，可通过下载 <span style="font-weight: bold;" class="bold">PGP 签名</span>（在[下载](https://archlinux.org/download/)页面的 *Checksums* 下方）到 ISO 文件所在的路径，然后用以下方式[验证签名](https://wiki.archlinuxcn.org/wiki/GnuPG#%E9%AA%8C%E8%AF%81%E7%AD%BE%E5%90%8D "GnuPG")：

```
$ gpg --keyserver-options auto-key-retrieve --verify archlinux-版本-x86_64.iso.sig
```

　　另外，在一台已经安装 Arch Linux 的计算机上可以通过以下方式验证：

```
$ pacman-key -v archlinux-版本-x86_64.iso.sig
```

　　**注意：** * 如果安装映像是从镜像站点下载，而不是从 [archlinux.org](https://archlinux.org/download/) 下载的话，其签名有被伪造的风险。在这种情况下，请您确保用来解码签名的公钥是被另一个可信的密钥签署的。`gpg`​ 命令将会输出公钥的指纹。

* 另一种验证签名的方法是确保公钥的指纹等于其中一位签署了 ISO 文件 [Arch Linux 开发者](https://archlinux.org/people/developers/)的指纹。请您参阅[维基百科](https://zh.wikipedia.org/wiki/%E5%85%AC%E5%BC%80%E5%AF%86%E9%92%A5%E5%8A%A0%E5%AF%86 "zhwp:公开密钥加密")获取更多关于公钥加密的信息。

### 准备安装介质 <span style="font-weight: bold;" class="bold">[</span>​[编辑](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&veaction=edit&section=4 "编辑章节：准备安装介质")  <span style="font-weight: bold;" class="bold">|</span>  [编辑源代码](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&action=edit&section=4 "编辑章节的源代码： 准备安装介质")]

　　Arch Linux 可以被制作成多种类型的安装介质，如 [U 盘](https://wiki.archlinuxcn.org/wiki/U_%E7%9B%98%E5%AE%89%E8%A3%85%E4%BB%8B%E8%B4%A8 "U 盘安装介质") 、[光盘](https://wiki.archlinuxcn.org/wiki/Optical_disc_drive#Burning "Optical disc drive")和带有 [PXE](https://wiki.archlinuxcn.org/wiki/Preboot_Execution_Environment "Preboot Execution Environment") 的网络安装映像。请您按照合适的文章与教程，使用所选映像为自己准备安装介质。

　　<u>注：本人使用VMware虚拟机安装，创建虚拟机在虚拟机光驱加载iso就好</u>

### 启动到 live 环境 <span style="font-weight: bold;" class="bold">[</span>​[编辑](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&veaction=edit&section=5 "编辑章节：启动到 live 环境")  <span style="font-weight: bold;" class="bold">|</span>  [编辑源代码](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&action=edit&section=5 "编辑章节的源代码： 启动到 live 环境")]

　　<u>注1：物理机的话进Bios找UEFI或者EFI选项</u>​<u>[禁用安全启动](https://wiki.archlinuxcn.org/wiki/UEFI/%E5%AE%89%E5%85%A8%E5%90%AF%E5%8A%A8#%E7%A6%81%E7%94%A8%E5%AE%89%E5%85%A8%E5%90%AF%E5%8A%A8 "UEFI/安全启动")</u>​<u>，或者从legacy模式启动安装</u>

　　<span style="font-weight: bold;" class="bold">注意：</span>  Arch Linux 安装镜像不支持 UEFI 安全启动（Secure Boot）功能。如果要引导安装介质，需要[禁用安全启动](https://wiki.archlinuxcn.org/wiki/UEFI/%E5%AE%89%E5%85%A8%E5%90%AF%E5%8A%A8#%E7%A6%81%E7%94%A8%E5%AE%89%E5%85%A8%E5%90%AF%E5%8A%A8 "UEFI/安全启动")。如果需要，可在完成安装后重新[配置](https://wiki.archlinuxcn.org/wiki/UEFI/%E5%AE%89%E5%85%A8%E5%90%AF%E5%8A%A8#%E5%AE%9E%E6%96%BD%E5%AE%89%E5%85%A8%E5%90%AF%E5%8A%A8 "UEFI/安全启动")。

　　​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071015662.png)​

1. 选择从带有 Arch 安装文件的介质启动，通常是需要在[计算机启动加电自检](https://zh.wikipedia.org/wiki/%E5%8A%A0%E7%94%B5%E8%87%AA%E6%A3%80 "zhwp:加电自检")时快速按下某个热键（比如部分主板会是F12键）。启动时的画面也可能会有提示，详情请参考自己的计算机的说明书或主板说明书。
2. 当引导加载程序菜单出现时，选择 *Arch Linux install medium* 并按 `Enter`​ 进入安装环境。  
    **提示：** 安装映像在 UEFI 模式下使用 [GRUB](https://wiki.archlinuxcn.org/wiki/GRUB "GRUB") 引导，在 BIOS 模式下使用 [syslinux](https://wiki.archlinuxcn.org/wiki/Syslinux "Syslinux") 引导。分别使用 `e`​ 或 `Tab`​ 来输入[引导参数](https://wiki.archlinuxcn.org/wiki/%E5%86%85%E6%A0%B8%E5%8F%82%E6%95%B0#%E9%85%8D%E7%BD%AE "内核参数")。请参阅 [README.bootparams](https://gitlab.archlinux.org/archlinux/mkinitcpio/mkinitcpio-archiso/blob/master/docs/README.bootparams) 获取[引导参数](https://wiki.archlinuxcn.org/wiki/%E5%86%85%E6%A0%B8%E5%8F%82%E6%95%B0#%E9%85%8D%E7%BD%AE "内核参数")列表。* 手动定义启动参数的一个常见例子是改变系统显示在超高分辨率（HiDPI）屏幕的字体的大小。为使系统在HiDPI屏幕上显示的字体有更好的可读性——当Live系统启动时屏幕还没有被识别为HiDPI的时候——使用 `fbcon=font:TER16x32`​ 会有帮助。参见 [HiDPI#Linux 控制台](https://wiki.archlinuxcn.org/wiki/HiDPI#Linux_%E6%8E%A7%E5%88%B6%E5%8F%B0 "HiDPI") 的详细解释。

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071015749.png)​
3. 您将会以 root 身份登录进入一个[虚拟控制台](https://en.wikipedia.org/wiki/Virtual_console "wikipedia:Virtual console")，默认的 Shell 是 [Zsh](https://wiki.archlinuxcn.org/wiki/Zsh "Zsh")。

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071015152.png)​

    如果要使用其他控制台（例如在安装时使用 [Lynx](https://lynx.invisible-island.net/lynx_help/Lynx_users_guide.html) 查看本指南），可以使用 `Alt+<i>箭头</i>`​ [快捷键](https://wiki.archlinuxcn.org/wiki/Keyboard_shortcuts "Keyboard shortcuts")切换不同的控制台。要[编辑](https://wiki.archlinuxcn.org/wiki/Help:Reading#%E6%B7%BB%E5%8A%A0%E3%80%81%E5%88%9B%E5%BB%BA%E3%80%81%E7%BC%96%E8%BE%91%E6%96%87%E4%BB%B6 "Help:Reading")配置文件，可以使用 [mcedit(1)](https://man.archlinux.org/man/mcedit.1)、[nano](https://wiki.archlinuxcn.org/wiki/Nano#%E4%BD%BF%E7%94%A8 "Nano") 和 [vim](https://wiki.archlinuxcn.org/wiki/Vim#%E7%94%A8%E6%B3%95 "Vim") 等文本编辑软件。请参阅 [packages.x86_64](https://geo.mirror.pkgbuild.com/iso/latest/arch/pkglist.x86_64.txt) 获取安装介质中包含的软件包列表。

4. 接下来先连接SSH客户端（不然要手打命令）

    输入下面命令设置ssh密码，随便设88888888都行

    ```bash
    passwd
    ```

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016297.png)​

    然后获取ip

    ```bash
    ip a
    ```

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071015914.png)​

    然后用ssh客户端输入上面拿到的ip地址，点连接，确定接受主机秘钥，输入账户root，密码是刚刚设置的密码8888888

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016746.png)​

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016795.png)​
5. 使用官方脚本安装

    命令：

    ```
    archinstall
    ```

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016436.png)​
6. 逐个配置

    ### Archinstall language

    安装器的语言，维持预设英文就好，因为tty也无法显示中文。

    ### Keyboard layout

    键盘设定，维持`us`​就行。

    ### Mirror region

    切换映射站点，进入后选取China的软体库(按空白键)，再按Esc返回

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016428.png)​

    ### Disk Configuration

    本人在这选的是use a best-effort那个选项

    选取要安装系统的磁盘，自行从容量判断。  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016023.png)​

    档案系统建议选BTRFS或EXT4，本人选BTRFS

    然后会有两个问题问你删除数据的问题全部默认yes就好

    ### Bootloader

    本人选择Systemd-boot

    archinstall指令稿的开机引导程式预设是使用Systemd-boot，此引导程式弹性不高，也可以改回传统的GRUB。

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016894.png)​

    ### SWAP

    RAM不足时拿硬碟分区来缓衝之用，维持预设让其自动建立。

    ### Hostname

    电脑名称，可维持预设。

    ### Root password

    设定Root密码，建议六位数以上。

    ### User account

    建立一般使用者。

    选取Add a user

    不设置就只有一个root账号，本人没设置

    输入新使用者的名称，建议小写字母，例如新增名叫`user`​的一般使用者，接著选取yes赋予其使用sudo的权限。

    选取Confrim and exit

    ### Profile

    这里可选取要將Arch安装为桌面电脑，还是伺服器的设定档。

    可以选取Desktop，桌面环境看个人选择，可以按照显示卡安装驱动。注意Nvidia的不要装到开源的nouveau，因其效能差又无法使用CUDA。

    也可以选最小安装，本人选的最小安装  
    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016604.png)​

    ### Audio

    默认没有

    ### Kernels

    要安装的核心变种，维持预设的`linux`​。除非你需要用Waydroid跑Android APP才选取`linux-zen`​核心。

    Arch Linux可依照用途，同时安装不同版本的Linux核心。

    ### Additional Packages

    额外套件。建议这边填入`noto-fonts-cjk`​装字体，不然开机中文字会变成方块。

    ### Network Configuration

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016405.png)​

    网路设定，如果前面选了桌面通常选2留给NetworkManager自动管理，没选桌面不要选2。

    选1安装后没网，要自己启用网卡配ip

    选3选网卡，选DHCP，安装好之后会自动获取ip，本人选的这个![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016158.png)​

    配置好选这个退出

    ### Timezone

    时区设定，默认就好。

    ### Automatic time sync

    开机自动校时，维持预设。

    ### Optional repositories

    额外的软体库。

    除非你需要用Wine跑Windows程式，才勾选`multilib`​开启32位元的软体库。

    设不设置都行

    ### 储存设定档

    可储存本次安装设定档供日后利用。

    选取Save Configuration

    选取Save all，它会將设定档储存到安装好的系统

    ### 开始安装

    確认一切无误后，选取Install开始安装，接著会按照以上设定档安装系统。因为上面选了KDE的设定档，下载与安装套件约需要半小时。

    装好后，选取No再输入Exit重开机。

    ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403071016562.png)​

　　‍
