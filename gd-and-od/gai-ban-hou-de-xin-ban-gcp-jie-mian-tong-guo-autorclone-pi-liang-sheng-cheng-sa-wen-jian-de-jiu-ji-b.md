# 改版后的新版GCP界面通过AutoRclone批量生成SA文件的究极保姆级教程

本文更新于2021.7.15。

准备工作：一台能访问Google的VPS。本文所使用的VPS环境为DigitalOcean的Ubuntu 20.04。

Debian 9+，Centos 7+ 理论可行，但未测试。

DigitalOcean注册地址：[https://m.do.co/c/76209bc8b100](https://m.do.co/c/76209bc8b100) （注册即可获得100刀的额度，可使用60天）

前言：自从GCP页面更改后，之前生成SA的教程都已失效，于是做一个教程帮助大家（本人能力有限，教程中难免有错误，还请各位谅解并指正）

![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/28-1-1024x511.png)

更改后的GCP授权页面

教程正式开始：

一. 创建项目并获取相关授权文件

1. 浏览器打开: [https://console.cloud.google.com/apis/library](https://console.cloud.google.com/apis/library)

![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/1-2.png)

2\. 第一次进入这个界面会弹出如上图的欢迎界面，同意之后点击同意并继续。

点击页面上方选择项目![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/2-2.png)

3\. 点击新建项目并填写相关信息（根据图示一步一步来）![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/3-1.png)![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/4-1.png)项目名称随意，点击创建![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/5-1.png)

此时，已经创建好了一个全新的项目，我们继续。

4\. 开启相关API

一般情况下需要开启如下5个API

```
Google Drive API
Identity and Access Management (IAM) API
Service Usage API
Cloud Resource Manager API
IAM Service Account Credentials API
```

我只示例Google Drive API开启具体步骤，其余API步骤相同：

在“搜索API和服务”框内输入 drive，点击Google Drive API。![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/6-1.png)![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/7-1.png)启用API

稍等片刻，Google Drive API就启动成功了。

然后点击左上角三条横线，找到 API与服务-库 搜索其余4个API并开启。（有的默认开启的就不用管了）

操作完成后，我们检验一下是否全部开启成功，

点击左上角三条横线，找到 API与服务-信息中心，在这里我们可以看到我们成功开启的所有API（除了刚刚开启的5个，其余的是默认开启的，不需要管）![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/11-1.png)

确认5个API开启后，我们进行下一步

5\. 配置OAuth同意屏幕并创建客户端（按照图示一步一步来）![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/12-1-1024x726.png)![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/13-1.png)点击外部-创建

应用名称、用户支持电子邮件、开发者联系信息随便填![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/14-1.png)![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/15-1.png)填好后保存并继续

测试用户这一栏不用填直接保存并继续。![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/16-1.png)

我们看到现在我们刚刚创建的应用还是测试版，测试版需要添加测试用户而且客户端ID和密码只有7天有效期，所以我们一定要发布应用，发布了之后客户端ID密码都是没有时间限制的。![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/17-1.png)

至此，OAuth同意屏幕配置完成，接下来创建客户端ID和密码

点击凭据，![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/18-1.png)

点击创建凭据-OAuth客户端 ID![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/19-1-1024x627.png)

应用类型选择桌面应用，名称随意，完成后点击创建，![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/20-1.png)

此时，客户端ID与密码创建完毕，可以记下来，以后使用方便，不想记也没事，以后还能看，![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/21-1.png)

点击下载授权文件，并改名为 credentials.json![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/GA601UT3LPW1VUVTB9-1-1024x294.png)

到此，我们成功获得了授权文件。

二、 通过AutoRclone项目创建SA

接下来需要到VPS上操作了，首先远程登陆好你的VPS。

1. 配置环境

Ubuntu、Debian:

```
apt-get update && apt-get upgrade -y
apt-get install wget curl python3 python3-pip git unzip screen sudo fuse -y
```

Centos:

```
yum -y update
yum install epel-release -y
yum install wget curl python3 python3-pip git screen unzip sudo fuse fuse-devel -y 
```

安装完成后，试试输入 python -V或者 python3 -V看看有没有反应，如果有说明 python 配置正确，请继续下一步；如果没有，请自行配置 python 环境。

2\. 安装Node.js

Debian:

```
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt-get install -y nodejs
```

Ubuntu:

```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
apt-get install -y nodejs
```

Centos:

```
curl -sL https://rpm.nodesource.com/setup_lts.x | bash -
yum install -y nodejs
yum install gcc-c++ make
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | tee /etc/yum.repos.d/yarn.repo
yum install yarn
```

安装好之后输入node -v，如果输出的是Node.js的版本号，则表示Node.js配置正确，若输出错误请自行配置Node.js环境

3\. 安装rclone、AutoRclone

Centos:

```
cd && curl https://rclone.org/install.sh | sudo bash
sudo git clone https://github.com/xyou365/AutoRclone && cd AutoRclone && sudo pip3 install -r requirements.txt
```

Ubuntu、Debian:

```
sudo apt-get install screen git && curl https://rclone.org/install.sh | sudo bash
sudo git clone https://github.com/xyou365/AutoRclone && cd AutoRclone && sudo pip3 install -r requirements.txt
```

4\. 将刚下载好的授权文件上传到AutoRclone根目录

上传可以使用宝塔面板或者xftp，这个不会的可以自行谷歌，

[https://www.google.com/search?q=ssh%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6](https://www.google.com/search?q=ssh%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6)

上传完成后继续操作，

5\. 生成SA

```
cd && cd /root/AutoRclone
python3 gen_sa_accounts.py --quick-setup 1 --new-only
```

这个命令的含义是创建一个项目并创建100个SA（对于99.9999%的人100个SA都够用了）![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/26-1024x132.png)

执行上条命令后会出现一个链接，复制并在浏览器中打开，登陆你刚刚创建项目的Google账号（注意别弄错了）进行授权，![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/23-2.png)点击高级-继续前往![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/24-2.png)赋予权限![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/25-2.png)复制返回码

复制返回码并粘贴到SSH终端，点击回车，等待SA生成完毕![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/26-1-1024x132.png)SA生成完毕![](https://wordpress.zzzshanshan.cf/wp-content/uploads/2021/07/27-1-1024x493.png)100个SA授权文件

我们可以执行 ls /root/AutoRclone/accounts ,发现里面有100个SA授权文件，至此SA已生成完毕。

温馨提示：SA授权文件非常重要，建议下载保存，丢失找回非常困难！
