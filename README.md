
# GitHub 仓库创建与 Sileo 源上传教程

本教程帮助您在 GitHub 上创建仓库，生成个人访问令牌，上传 `.deb` 文件到 GitHub，并将其添加到 Sileo 中。

### 注意本教程以 [shaoxia] 仓库名称为例，底下涉及此名称皆可自行修改
---

## 创建 GitHub 仓库

1. 在浏览器中打开 [GitHub](https://github.com/) 并登录您的账户。
2. 点击右上角头像，选择 `+ Create new` 然后选择`New repository`。（创建新仓库）
3. 在 `Repository name` 中输入仓库名称（如 `shaoxia`），确认名称可用（会出现绿色对勾）。
4. 选择 `Public` 作为仓库类型（公开仓库），然后点击 `Create repository`。（创建仓库）
5. 会跳转到新页面,在新页面中，点击 `creating a new file` 创建文件。（创建新文件）
6. 在编辑页面粘贴以下内容：

   ```plaintext
   Origin: - 少侠的源(可自行修改)
   Label: - 少侠的源(可自行修改)
   Description: 本人自用, 收藏备份。(可自行修改)
   Suite: stable
   Version: 1.0
   Architectures: iphoneos-arm64
   Components: main

7.	然后在页面最上方将文件命名为 shaoxia/Release，然后点击 Commit changes。（提交更改）
8.	在仓库页面点击 ... > Settings，进入 Pages。（仓库设置 > 页面）
9.	在 Build and deployment(构建和部署)下选择 main，点击 Save(存储)
10.	等待 2 分钟，刷新页面。找到 GitHub Page 下的链接并复制，作为源地址添加到 Sileo 进行使用。

## 创建 GitHub 个人访问令牌

1.	点击 GitHub 网站右上角头像 > Settings。（设置）
2.	在其中找到并依次点击它 `Developer settings (开发者设置)>`Personal access tokens (个人访问令牌 )>`Generate new token（生成新令牌）

3.	输入描述性名称（如 iOS 开发），至少选择 repo 权限。（设置令牌权限）(也可以全部选择)也就是全部打上✅

4.	点击 Generate token(生成令牌)并将生成的令牌复制到剪贴板，自行进行保存（页面关闭后无法再次查看）。

### 安装所需插件
1.打开 Sileo，搜索并安装以下插件：

     -  NewTerm 3 Beta （命令行终端）
     -  git （版本控制工具）
     -  dpkg-dev （Debian 软件包开发工具）
     -  dpkg-repack （重新打包已安装的 .deb 文件）
     -  libdpkg-dev （dpkg 开发库）
     -  openssh （SSH 连接工具）

### 克隆 GitHub 仓库
1. 查看仓库HTTPS链接

 - 在自己的GitHub仓库主页。页面中央偏右的位置
 - 会看到一个标有“Code”（代码）的绿色按钮。
 - 点击该按钮旁边的倒三角图标，选择“HTTPS”选项，
 - 然后复制显示在下方的URL链接。复制保存下来，等下要用



2.	打开 NewTerm 3 Beta，输入以下命令将仓库克隆到本地
（此处替换为自己的 HTTPS 链接，将仓库克隆到本地）


         git clone https://github.com/liym5238/shaoxia.git


2.	克隆完成后，输入
         
         pwd 

 - 命令查看本地路径如(/var/jb/var/mobile)
 - 使用 Filza 文件管理器进入此路径，确认 shaoxia 文件夹已创建。

3.	确认克隆成功后再用命令绑定 Git 用户信息：

         git config --global user.name "你的用户名"
         git config --global user.email "你的邮箱"

（绑定 Git 用户名和邮箱）

生成 Packages 文件

1.	在 shaoxia 文件夹中创建 debs 文件夹。
2.	将 .deb 插件包复制到 shaoxia/debs 文件夹中。
3.	在 NewTerm 3 Beta 中，输入以下命令生成 Packages 文件：

         cd /var/jb/var/mobile/此处替换你的仓库名称/debs
         dpkg-scanpackages . /dev/null > ../Packages
（生成 Packages 文件）

4.	使用 Filza 查看 shaoxia 文件夹下的 Packages 文件，确保内容显示 .deb 插件包的信息。（确认文件内容）

### 更新仓库到 GitHub

1.	在 NewTerm 3 Beta 中，输入以下命令初始化 Git 并将文件推送到 GitHub：

         cd /var/jb/var/mobile/shaoxia
         git pull
         git add .
         git commit -m "xxx"
         git push
         
（添加、提交并推送到 GitHub）如没反应请按回车

2. 系统可能会提示输入 GitHub 用户名和令牌在看到

提示输入 `Username 的时候输入你的用户名到终端中ps(键盘切换到英文键盘)

提示输入 `Password 时在备忘录复制你的令牌粘贴回车(令牌在终端中不会显示)

3.	完成上传后，在 Sileo 刷新源并验证是否成功添加 .deb 文件。

4.设置上传文件大小限制命令(只需要配置一次)设置为500m

    
    git config --global http.postBuffer 524288000 


## GitHub SSH Key 配置指南

本教程将帮助您在 GitHub 上配置 SSH 密钥、克隆仓库，并使用 SSH 上传更改。

步骤 1: 生成 SSH Key

在终端中输入以下命令，将 your_email@example.com 替换为您的 GitHub 账号邮箱。连续按四五次回车以使用默认配置。

      ssh-keygen -t ed25519 -C "your_email@example.com"

步骤 2: 复制公钥内容

	1.	打开 Filza 文件管理器，导航至 /var/jb/var/mobile/.ssh。
	2.	找到 id_rsa.pub 文件，打开并复制文件的全部内容。

步骤 3: 在 GitHub 添加公钥

	1.	登录 GitHub，进入 Settings > SSH and GPG keys。
	2.	点击 New SSH key 按钮，将之前复制的公钥内容粘贴到文本框中，保存。

步骤 4: 验证 SSH Key 配置

在终端中输入以下命令，验证是否成功配置 SSH：

      ssh -T git@github.com

成功配置后将看到类似如下的提示：

      Hi liym5238! You've successfully authenticated, but GitHub does not provide shell access.

步骤 5: 克隆仓库并配置 SSH URL

1.	使用 SSH 链接重新克隆仓库。
2.	打开克隆的仓库目录下的 .git/config 文件，将 url =  后的 HTTPS 链接替换为 SSH 格式的链接：

          url = git@github.com:liym5238/liym.git



步骤 6: 使用 SSH 命令上传到 GitHub

在本地仓库根目录（例如 /var/jb/var/mobile/liym）中执行以下命令以推送更改到 GitHub：

      cd /var/jb/var/mobile/liym
      git pull
      git add .
      git commit -m "提交说明"
      git push

按照上述步骤，您现在可以通过 SSH 安全地将代码推送到 GitHub。


7、一键替换链接sh文件，sh文件权限记得更改为0775权限

    #!/bin/bash

    # 定义要查找和替换的字符串
    old_url="https://trollstorex.github.io/sileodepiction/repo/banner.png"
    new_url="https://liym5238.github.io/liym/icon/hengfu.png"

    # 查找当前目录及其子目录中的所有 JSON 文件，并替换旧链接为新链接
    find . -name "*.json" -type f -exec sed -i '' "s|$old_url|$new_url|g" {} +

    echo "链接替换完成。"


</details>



