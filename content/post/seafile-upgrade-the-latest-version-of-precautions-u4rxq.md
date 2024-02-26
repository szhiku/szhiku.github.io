---
title: seafile升级最新版注意事项（11.05版本为例）
slug: seafile-upgrade-the-latest-version-of-precautions-u4rxq
url: /post/seafile-upgrade-the-latest-version-of-precautions-u4rxq.html
date: '2024-02-20 22:08:33'
lastmod: '2024-02-20 22:22:20'
toc: true
tags:
  - 运维
  - Linux
  - seafile
categories:
  - 技术
keywords: 运维,Linux,seafile
isCJKLanguage: true
---

# seafile升级最新版注意事项（11.05版本为例）

　　笔者以当前最新版11.05为例，以下内容为亲测

　　‍

　　1、docker-compose.yml文件不能使用seafileltd/seafile-mc:latest，得用seafileltd/seafile-mc:11.05，具体原因不明，seafileltd/seafile-mc:latest现在是8.07的版本

　　2、升级后会发现无法进入管理页面，此时需要添加到[相应的 Django 设置](https://docs.djangoproject.com/en/4.2/ref/settings/#csrf-trusted-origins)中`conf/seahub_settings.py`​：

　　参考：[更新到 Seafile 11.0.0 后，docker 的 CSRF 验证失败 · Issue #2707 · haiwen/seafile · GitHub](https://github.com/haiwen/seafile/issues/2707)

```bash
CSRF_TRUSTED_ORIGINS = ["https://seafile.example.com"]
```

　　使用上面参考提到的 docker-compose 文件，该文件位于容器外部的`/home/me/seafile/data/seafile/conf/seahub_settings.py`​，在该文件最后新增一行`CSRF_TRUSTED_ORIGINS = ["https://seafile.example.com"]`​

　　暂时没有碰到其他问题
