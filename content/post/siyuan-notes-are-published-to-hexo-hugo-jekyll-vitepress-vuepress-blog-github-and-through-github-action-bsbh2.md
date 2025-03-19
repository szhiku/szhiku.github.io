---
title: >-
  思源笔记一键发布至Hexo、Hugo、Jekyll、Vitepress、Vuepress博客（github）并通过github
  action构建page并同步gitee page
slug: >-
  siyuan-notes-are-published-to-hexo-hugo-jekyll-vitepress-vuepress-blog-github-and-through-github-action-bsbh2
url: >-
  /post/siyuan-notes-are-published-to-hexo-hugo-jekyll-vitepress-vuepress-blog-github-and-through-github-action-bsbh2.html
date: '2024-02-19 03:06:22'
lastmod: '2024-02-19 13:32:15'
toc: true
tags:
  - 思源笔记
  - 运维
categories:
  - 技术
keywords: 思源笔记,运维
isCJKLanguage: true
---



# 思源笔记一键发布至Hexo、Hugo、Jekyll、Vitepress、Vuepress博客（github）并通过github action构建page并同步gitee page

　　先决条件：思源笔记、一键发布工具、Hexo\Hugo\Jekyll\Vitepress\Vuepress相关主题

　　以Hugo的[hugo-theme-next](https://github.com/hugo-next/hugo-theme-next-starter)主题示例，其他的同理，构建文件不同

　　注意事项：Hugo使用无序和有序列表尽量不要超过3层

　　部署和配置时长：1小时+

　　部署流程：克隆Hugo主题至自己仓库->创建主页仓库->获取github个人访问令牌token->github action构建并输出至主页主页->gitee导入github仓库并配置镜像仓库->开通gitee page->配置[hugo-theme-next](https://github.com/hugo-next/hugo-theme-next-starter)

　　使用流程：思源笔记一键发布插件配置->使用一键发布插件发布文章并选择hugo

　　部署详情：

* 克隆Hugo主题至自己仓库

  * ## ⏬ 克隆主题

    点击右上角的 `Use this template`​​ 绿色按钮然后填写代码仓库的相关信息，参考如下：

    ![使用模板创建](assets/net-img-68747470733a2f2f696d67732e6c6973656e6875692e636e2f6875676f2d6e6578742f7573652d6875676f2d6e6578742d737461727465722e706e67-20240219134347-1yf61br.png)[https://camo.githubusercontent.com/0031520ad9bb551333a37cf756fbfb3f538e6f33638cd3318e5d70f94aae28e8/68747470733a2f2f696d67732e6c6973656e6875692e636e2f6875676f2d6e6578742f7573652d6875676f2d6e6578742d737461727465722e706e67](https://camo.githubusercontent.com/0031520ad9bb551333a37cf756fbfb3f538e6f33638cd3318e5d70f94aae28e8/68747470733a2f2f696d67732e6c6973656e6875692e636e2f6875676f2d6e6578742f7573652d6875676f2d6e6578742d737461727465722e706e67)

    最后点击 `Create repository from template`​​ 绿色按钮，会直接在你的空间中生成站点代码，再使用`git clone`​​命令把它克隆到本地进行创作。

    记得首次完全克隆后，需要在根目录中使用如下的 `Git`​​ 子模块更新命令拉取 `hugo-theme-next`​​ 主题的最新版本。
* 创建主页仓库

  * 自定义首页，首先需要创建一个与你 Github ID 同名的仓库

    ![image](assets/image-20240219134246-p234dqf.png)

    ​​

    创建完成后就可以开始为你的首页添加一些有趣的内容了，代码格式可以是 `markdown`​ 语法，也可以是 `HTML`​ 语法，但 HTML 的扩展性更强一点，因此笔者使用的是 HTML 语法
* 获取github个人访问令牌token

  当您需要在GitHub Action中执行需要身份验证的操作时，例如推送到另一个仓库或访问私有仓库，您需要提供身份验证凭据。GitHub提供了一种安全的方式来存储这些凭据，即使用Personal Access Token（个人访问令牌）并将其添加到仓库的Secrets中。

  以下是如何生成Personal Access Token并将其添加到GitHub仓库的Secrets中的详细步骤：

  1. **生成Personal Access Token**：

  ```text
  - 登录到您的`GitHub`账户。

  - 点击右上角的头像，然后选择`Settings`（设置）。

  - 在左侧导航栏中，点击`Developer settings`（开发者设置）。

  - 在下拉菜单中，选择`Personal access tokens`（个人访问令牌）。

  - 点击右上角的`Generate token`（生成令牌）按钮。
  ```

  ![](assets/net-img-v2-d8374af8e1d58757770b79ebea8d6165_720w-20240219134347-di0w4pu.webp)

  2. **配置Personal Access Token的权限**：

  ```text
  当您生成`Personal Access Token`时，需要为其分配适当的权限。根据您的需求，可以为其分配不同的权限，例如访问仓库、读取用户资料等。请谨慎选择权限，最小化所需的权限以提高安全性。
  ```

  ![](assets/net-img-v2-ffd1fae6f18feaf9a4dc8cd2677f8373_720w-20240219134347-dh1j1we.webp)

  3. **生成令牌**：

  ```text
  在配置了权限后，滚动到页面底部，然后单击`Generate token`（生成令牌）。`GitHub`将生成一个令牌，并将其显示在屏幕上。请务必将此令牌复制到安全的地方，因为在生成后，您将无法再次查看完整的令牌。
  ```

  4. **将令牌添加到GitHub仓库的Secrets**：

  ```text
  - 转到包含您的博客源文件的GitHub仓库。

  - 在仓库顶部导航栏中，单击`Settings`（设置）。

  - 在左侧导航栏中，选择`Secrets`（密码）。

  - 单击`New repository secret`（新存储库密码）按钮。

  - 在`Name`字段中，输入`PERSONAL_TOKEN`（或您选择的名称）。

  - 在`Value`字段中，粘贴您在第3步中生成的`Personal Access Token`。

  - 单击`Add secret`（添加密码）按钮。
  ```

  ![](assets/net-img-v2-1c608b3468765b402d771d6b2766533d_720w-20240219134347-0tj3tzd.webp)

  ![](assets/net-img-v2-a0e5a222dcb3eee33a2d9feb6958bdcc_720w-20240219134347-rxafbvf.webp)

  ![](assets/net-img-v2-4a269aef8ba5a58672759e584a2a7f37_720w-20240219134347-qryzvzk.webp)

  现在，您的`Personal Access Token`​已经以安全的方式存储在`GitHub`​仓库的`Secrets`​中，并可以在`GitHub Action`​的工作流程中使用。

  在上文中提到的`PERSONAL_TOKEN`​将会被引用并传递给GitHub Action中的相关步骤，以便执行需要身份验证的操作。这个过程确保了敏感凭据的安全性，并且不会将它们直接硬编码到工作流程文件中，从而保护了您的GitHub仓库的安全性。
* github action构建并输出至主页主页

  * 配置GitHub Action，首先需要在博客源文件的仓库中创建一个`.github/workflows`​目录，并在该目录下创建一个`.yml`​配置文件。我们将其命名为`deploy.yml`​。

    以下是本人改进过的自动化部署Hugo博客的示例`deploy.yml`​配置文件：
  * .github/workflows/deploy.yml

    ```
    name: deploy

    on:
      push:
        branches:
          - main  # 或者是你的源代码分支

    jobs:
      deploy:
        runs-on: ubuntu-latest

        steps:
        - name: Checkout repository
          uses: actions/checkout@v4
          with:
              submodules: true
              fetch-depth: 0

        - name: Set up Hugo
          uses: peaceiris/actions-hugo@v2.6.0
          with:
            hugo-version: "latest"
            extended: true

        - name: Build and Deploy
          run: |
            hugo -F --cleanDestinationDir  # 生成静态文件
            mkdir -p public  # 确保public文件夹存在
            cp -r public/* ./  # 复制生成的静态文件到仓库根目录

        - name: Deploy to GitHub Pages
          uses: peaceiris/actions-gh-pages@v3.9.3
          with:
            PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}  # 你的个人访问令牌
            EXTERNAL_REPOSITORY: szhiku/szhiku.github.io  # 你的GitHub Pages仓库
            PUBLISH_BRANCH: main  # 或者是你的GitHub Pages分支
            PUBLISH_DIR: ./
            commit_message: ${{ github.event.head_commit.message }}
    ```
  * 之后点右上角的commit（提交），会自动触发构建，构建详情如下图

    ![](assets/net-img-202402191302174-20240219134421-tqdod40.png)​​​
  * 正常的话，github pages已经可以了

    点击设置，再点击右边栏上的 Pages

    ![](assets/net-img-90d49c1edee7248ec11340d254384615-20240219134421-c6eav4e.png)

    5. 设置 main 分支，root 根文件夹，并点击保存

    ![](assets/net-img-e35c83245050bff365d075ea16373240-20240219134421-yohzmue.png)

    6. 可以看到 Actions 中已经在构建和部署页面了

    ![](assets/net-img-b0bf870906f11bb045517a7594fb9fea-20240219134421-yr69oif.png)

    7. 构建完成后，重新回到 设置的Pages 页，可以看到多了如下的提示，点击查看即可

    ![](assets/net-img-73c1d6dd1243661a32e6b171bd05e937-20240219134421-bc1un35.png)
* gitee导入github仓库并配置镜像仓库

  * 登录gitee导入github仓库

    ![](assets/net-img-202402191312616-20240219134421-8k71xcy.png)
  * 添加 Pull 方向的镜像

    Pull 方向的镜像用于将 `GitHub`​ 的仓库镜像到 `Gitee`​ 。

    > 你可以根据自身需求选择 自动镜像 或 手动镜像。
    >

    你可以通过以下方式配置 Pull 方向的镜像：

    1. 进入需要使用镜像功能的仓库，进入「管理」找到「仓库镜像管理」选项，点击「添加镜像」按键；

        > 如果你还没有绑定 GitHub 帐号，请根据弹窗提示绑定 GitHub 帐号；
        >

        ![输入图片说明](assets/net-img-110626_76a3ab9b_8249553-20240219134422-lpenit9.png "12.png")
    2. 添加镜像；  
        ​![输入图片说明](assets/net-img-111013_4806d22e_8249553-20240219134422-xsfb6n3.png "13.png")

        1. 在「镜像方向」中选择 Pull 方向；
        2. 在「镜像仓库」下拉列表中选择需要镜像的仓库；
        3. 在「个人令牌」中输入你的 [GitHub 私人令牌](https://help.gitee.com/repository/settings/sync-between-gitee-github#%E5%A6%82%E4%BD%95%E7%94%B3%E8%AF%B7-github-%E7%A7%81%E4%BA%BA%E4%BB%A4%E7%89%8C)；

            * 私人令牌中必须包含对 `repo`​ 的访问授权，否则添加后镜像不可用；
        4. 根据自身需求选择是否勾选「自动从 GitHub 同步仓库」；

            * 勾选后，我们将会在镜像仓库中自动生成 webhook 用于实现自动镜像；
            * 此功能需要你的个人令牌中包含对 `admin:repo_hook`​ 的访问授权，否则会添加失败；
        5. 点击「添加」保存镜像配置；

            * 如果添加失败，请根据 [如何申请 GitHub 私人令牌](https://help.gitee.com/repository/settings/sync-between-gitee-github#%E5%A6%82%E4%BD%95%E7%94%B3%E8%AF%B7-github-%E7%A7%81%E4%BA%BA%E4%BB%A4%E7%89%8C) 提供的流程重新申请私人令牌；
            * 如果重新申请私人令牌后仍然添加失败，请取消勾选「自动从 GitHub 同步仓库」后点击「添加」保存镜像，并 [手动配置 webhook](https://help.gitee.com/repository/settings/sync-between-gitee-github#%E5%A6%82%E4%BD%95%E6%89%8B%E5%8A%A8%E9%85%8D%E7%BD%AE-webhook)。

    配置完成后，可以通过以下方式触发镜像操作（Gitee 从 GitHub 同步仓库）：

    * 推送代码到 GitHub 镜像仓库
    * [手动更新镜像](https://help.gitee.com/repository/settings/sync-between-gitee-github#%E6%89%8B%E5%8A%A8%E6%9B%B4%E6%96%B0)

    > 镜像触发的最短间隔时间为 5 分钟。
    >

    > 如果只配置了 Pull 方向的镜像，建议你将最新的代码提交到 GitHub 镜像仓库；
    >
    > Gitee 会自动从 GitHub 同步仓库（分支/Branches、标签/Tags、提交记录/Commits）。
    >
* 开通gitee page

  * **A.新建仓库 test_pages**

    ![输入图片说明](assets/net-img-26173338_Pmcg-20240219134422-shk3ffc.png)

    点击创建完成仓库的创建

    **B.添加文件** **​`index.html`​**​  **(注意名称是 index.html 哦！)**

    点击新建文件

    ![输入图片说明](assets/net-img-26172523_5GI8-20240219134422-6dx839q.png)

    文件名输入`index.html`​，内容就是简单的`html`​

    ![输入图片说明](assets/net-img-26173106_Jn2d-20240219134423-q70k0xn.png)

    点击提交，将文件提交到仓库

    **C.选择 pages 服务**

    ![输入图片说明](assets/net-img-26173423_zzeF-20240219134423-ptu674m.png)

    **D.选择需要部署的分支，这里选择 Master 启动服务。**

    ![输入图片说明](assets/net-img-26173508_e3TE-20240219134423-6g1nv74.png)

    **E.访问生成的网站地址，即可以查看你部署的静态页面啦！**

    ![输入图片说明](assets/net-img-26173825_h9D1-20240219134423-3ev7gxk.png) ![输入图片说明](assets/net-img-26173847_USPU-20240219134423-0r3pfoj.png)

    ### 3. 已经有 Pages 仓库如何部署到 Gitee 的 Pages

    以`jQuery-File-Upload`​仓库为例，仓库地址：[https://github.com/blueimp/jQuery-File-Upload](https://github.com/blueimp/jQuery-File-Upload)

    它在 Github 上的 Pages 地址是：[https://blueimp.github.io/jQuery-File-Upload/](https://blueimp.github.io/jQuery-File-Upload/)

    如果想把它转移到 Gitee Pages，只需要登录你的 Gitee 账户，点击右上角的 `+`​ 号，选择新建仓库

    ![输入图片说明](assets/net-img-26174500_j9HQ-20240219134423-xpjs2uk.png)

    ![输入图片说明](assets/net-img-26174556_lc6V-20240219134423-cobl7ux.png)

    ![输入图片说明](assets/net-img-26174630_Kpri-20240219134423-4kpmkb9.png)

    然后点击创建，仓库会在后台自动导入，导入成功后，点击菜单栏的服务下拉`Gitee Pages`​

    ![输入图片说明](assets/net-img-26175015_PomW-20240219134423-nns3gvu.png)

    ![输入图片说明](assets/net-img-26175207_KKZ0-20240219134423-lwqky50.png)

    这里我们默认的`Pages`​服务分支是仓库的默认分支，但是你也已选择自己静态页面所在的分支，这里`jQuery-File-Upload`​仓库的静态页面分支是`gh-pages`​，选择`gh-pages`​并点击启动服务。

    ![输入图片说明](assets/net-img-26175333_xxzm-20240219134424-ee0yxiw.png) 至此，静态网页已经部署成功，访问提供的地址：[https://frech.gitee.io/jquery-file-upload/](https://frech.gitee.io/jquery-file-upload/) 即可查看到`jQuery-File-Upload`​仓库的静态官网。

    ![输入图片说明](assets/net-img-26175421_ikZP-20240219134424-7z7efoe.png)
* 配置[hugo-theme-next](https://github.com/hugo-next/hugo-theme-next-starter)

  * 来到这个文件夹

    ![](assets/net-img-202402191326427-20240219134424-czvthq8.png)
  * 按照备注说明依次修改配置文件config.yaml、menus.yaml、params.yaml，修改后记得点击右上角绿色的commt，之后会自动构建

    ![](assets/net-img-202402191326204-20240219134424-wtmsi8q.png)

　　使用详情：

* 思源笔记一键发布插件配置

  ![image](assets/image-20240219132807-j71e65k.png)

  ![image](assets/image-20240219132847-sjv5p59.png)

  ![image](assets/image-20240219133012-jo8yqb4.png)
* 使用一键发布插件发布文章并选择hugo

  ![image](assets/image-20240219133033-c400yl4.png)

  ![image](assets/image-20240219133114-eo3gmsd.png)

　　参考资料：

　　[教你如何自定义 ](https://www.baidu.com/link?url=Yrp6bIWKiFy1pBySoveNjB5hAtjHEX30sU_KYlUSmIH_xOdPvyqLZGDRecX-DuVzvvdYU-8CMXWLBSt7cBiCGKHtC4O77RSgMNvACNAtSw7&wd=&eqid=e8eeb3c3000530ad0000000665d2de65)​*[Github 首页](https://www.baidu.com/link?url=Yrp6bIWKiFy1pBySoveNjB5hAtjHEX30sU_KYlUSmIH_xOdPvyqLZGDRecX-DuVzvvdYU-8CMXWLBSt7cBiCGKHtC4O77RSgMNvACNAtSw7&wd=&eqid=e8eeb3c3000530ad0000000665d2de65)*​[_设置](https://www.baidu.com/link?url=Yrp6bIWKiFy1pBySoveNjB5hAtjHEX30sU_KYlUSmIH_xOdPvyqLZGDRecX-DuVzvvdYU-8CMXWLBSt7cBiCGKHtC4O77RSgMNvACNAtSw7&wd=&eqid=e8eeb3c3000530ad0000000665d2de65)​*[github主页](https://www.baidu.com/link?url=Yrp6bIWKiFy1pBySoveNjB5hAtjHEX30sU_KYlUSmIH_xOdPvyqLZGDRecX-DuVzvvdYU-8CMXWLBSt7cBiCGKHtC4O77RSgMNvACNAtSw7&wd=&eqid=e8eeb3c3000530ad0000000665d2de65)*​[-CSDN博客](https://www.baidu.com/link?url=Yrp6bIWKiFy1pBySoveNjB5hAtjHEX30sU_KYlUSmIH_xOdPvyqLZGDRecX-DuVzvvdYU-8CMXWLBSt7cBiCGKHtC4O77RSgMNvACNAtSw7&wd=&eqid=e8eeb3c3000530ad0000000665d2de65)

　　*[【Hugo网站搭建】GitHub](http://www.baidu.com/link?url=-iBiQYv9NjBjX8204N9vgZMP3MLIBTc3bpQbYNy0-chX95xjJslTULHrUfGy5rWQ)*​[ ](http://www.baidu.com/link?url=-iBiQYv9NjBjX8204N9vgZMP3MLIBTc3bpQbYNy0-chX95xjJslTULHrUfGy5rWQ)​*[Action自动化部署Hugo博客](http://www.baidu.com/link?url=-iBiQYv9NjBjX8204N9vgZMP3MLIBTc3bpQbYNy0-chX95xjJslTULHrUfGy5rWQ)*​[ - 知乎](http://www.baidu.com/link?url=-iBiQYv9NjBjX8204N9vgZMP3MLIBTc3bpQbYNy0-chX95xjJslTULHrUfGy5rWQ)

　　*[仓库镜像](https://www.baidu.com/link?url=Ejh-KorXyr7zUaawPULt2yLIapJp6JfJ9KLP5jzUGg3Luk8bcB4A3VBb3q4H0A3GNO4fECHi8WF7l-FD_tcJe0YzVSyFxOreSDccBvbI1gG&wd=&eqid=f9b446230021d1900000000665d2e299)*​[管理(](https://www.baidu.com/link?url=Ejh-KorXyr7zUaawPULt2yLIapJp6JfJ9KLP5jzUGg3Luk8bcB4A3VBb3q4H0A3GNO4fECHi8WF7l-FD_tcJe0YzVSyFxOreSDccBvbI1gG&wd=&eqid=f9b446230021d1900000000665d2e299)​*[Gitee](https://www.baidu.com/link?url=Ejh-KorXyr7zUaawPULt2yLIapJp6JfJ9KLP5jzUGg3Luk8bcB4A3VBb3q4H0A3GNO4fECHi8WF7l-FD_tcJe0YzVSyFxOreSDccBvbI1gG&wd=&eqid=f9b446230021d1900000000665d2e299)*​[&lt;-&gt;Github 双向同步) | Gitee 产品文档](https://www.baidu.com/link?url=Ejh-KorXyr7zUaawPULt2yLIapJp6JfJ9KLP5jzUGg3Luk8bcB4A3VBb3q4H0A3GNO4fECHi8WF7l-FD_tcJe0YzVSyFxOreSDccBvbI1gG&wd=&eqid=f9b446230021d1900000000665d2e299)

　　*[Gitee](https://www.baidu.com/link?url=4yI1ffnBG_5wKci_PewuB10LnLs4mokmprV_cgfIKWXF-oCCwDHHaHgZNC62UGDX&wd=&eqid=ab5622a70011e1580000000665d2e431)*​[ ](https://www.baidu.com/link?url=4yI1ffnBG_5wKci_PewuB10LnLs4mokmprV_cgfIKWXF-oCCwDHHaHgZNC62UGDX&wd=&eqid=ab5622a70011e1580000000665d2e431)​*[Pages](https://www.baidu.com/link?url=4yI1ffnBG_5wKci_PewuB10LnLs4mokmprV_cgfIKWXF-oCCwDHHaHgZNC62UGDX&wd=&eqid=ab5622a70011e1580000000665d2e431)*​[ | Gitee 产品文档](https://www.baidu.com/link?url=4yI1ffnBG_5wKci_PewuB10LnLs4mokmprV_cgfIKWXF-oCCwDHHaHgZNC62UGDX&wd=&eqid=ab5622a70011e1580000000665d2e431)

　　‍
