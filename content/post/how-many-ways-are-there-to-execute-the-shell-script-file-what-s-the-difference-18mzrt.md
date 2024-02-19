---
title: 执行shell脚本文件有多少种方法？有什么区别？
slug: >-
  how-many-ways-are-there-to-execute-the-shell-script-file-what-s-the-difference-18mzrt
url: >-
  /post/how-many-ways-are-there-to-execute-the-shell-script-file-what-s-the-difference-18mzrt.html
date: '2024-02-19 22:30:21'
lastmod: '2024-02-19 22:33:16'
toc: true
tags:
  - Linux
  - 运维
categories:
  - 技术
keywords: Linux,运维
isCJKLanguage: true
---

# 执行shell脚本文件有多少种方法？有什么区别？

　　执行`.sh`​文件有几种方法，主要包括：

1. <span style="font-weight: bold;" class="bold">直接运行</span>：

    ```bash
    ./your_script.sh
    ```

    这种方式需要在脚本文件的目录下执行，并确保脚本文件有执行权限 (`chmod +x your_script.sh`​)。这种方式的路径解析是相对于当前工作目录的。
2. <span style="font-weight: bold;" class="bold">通过bash解释器运行</span>：

    ```bash
    bash your_script.sh
    ```

    或者

    ```bash
    sh your_script.sh
    ```

    这种方式不需要执行权限，会使用指定的解释器来运行脚本。如果你使用`bash`​或`sh`​关键字，可以确保脚本在不同的环境中都能运行。
3. <span style="font-weight: bold;" class="bold">通过source命令运行</span>：

    ```bash
    source your_script.sh
    ```

    或者简写为：

    ```bash
    . your_script.sh
    ```

    这种方式会在当前shell环境中执行脚本，而不是启动一个新的进程。这意味着脚本中的变量和函数等将在当前shell中生效。

　　这几种方法的主要区别在于执行环境和作用域。直接运行和通过bash解释器运行都会创建一个新的进程，而source命令则在当前shell中执行，因此会影响当前环境。选择执行方式取决于你的需求，如果脚本需要修改当前shell的环境变量或执行其他会影响当前环境的操作，建议使用source命令。
