# rclone/fclone安装教程

准备工作\



1.一个谷歌账号

2.一个Google Team Drive帐户，网上很多，当然也可以自己建一个团队盘。\


## **详细过程**

**安装python3和AutoRclone--生成服务账号--将service accounts加入Google Groups--安装gclone复制文件**

## 安装Python3

{% tabs %}
{% tab title="windows" %}
去官网（https://www.python.org/downloads/）找到你的安装包，直接下载安装。

![](<../.gitbook/assets/image (26).png>)

![](<../.gitbook/assets/image (15).png>)
{% endtab %}

{% tab title="linux" %}
linux安装python3比较简单，一般自带的

可以`command -v python3`测试一下
{% endtab %}
{% endtabs %}

## 安装Rclone

{% tabs %}
{% tab title="windows" %}
1、安装最新的rclone，直接从Github([https://rclone.org/downloads/](https://rclone.org/downloads/))下载解压，设置系统变量即可&#x20;

2、安装autoclone([https://github.com/xyou365/AutoRclone](https://github.com/xyou365/AutoRclone))，下载解压备用，cmd挂代理后运行以下命令：

```
cd C:\iApp\AutoRclone-master
pip3 install -r requirements.txt
#有可能提示更新，根据提示运行升级命令即可
```

> 系统变量设置方法：
>
> 1. 在“搜索”中，搜索以下内容并进行选择：控制面板
> 2. 单击高级系统设置链接。
> 3. 单击环境变量。在系统变量部分中，找到并选择 PATH 环境变量。单击编辑。如果 PATH 环境变量不存在，请单击新建。比如你的Rclone.exe在目录D:\Rclone\Rclone.exe， 则你的 PATH 环境量的值即为D:\Rclone
> 4. 在编辑系统变量（或新建系统变量）窗口中，指定 PATH 环境变量的值。单击确定。通过单击确定关闭所有剩余窗口。

![](<../.gitbook/assets/image (16).png>)


{% endtab %}

{% tab title="linux" %}
linux同样比较简单

1、安装rclone使用官方脚本：

```
curl https://rclone.org/install.sh | sudo bash
```

2、安装autorclone

```
sudo git clone https://github.com/xyou365/AutoRclone && cd AutoRclone && sudo pip3 install -r requirements.txt
```
{% endtab %}
{% endtabs %}

## **生成SA**

本步骤，windows和linux是一样的，如果你之前没创建过项目, 直接运行

`python gen_sa_accounts.py --quick-setup 1`

若输入python 没反应，请改一下python3或者 py3

解释一下

* 创建1个项目（项目0到项目5）
* 开启相关的服务
* 创建100个service accounts
* 将100个service accounts的授权文件下载到accounts文件夹下面

若第一次运行会让你开启权限  ![](https://i0.hdslb.com/bfs/article/0fa381d44e9eba59bfc04e51d1f2387c13bfdc3b.png@942w\_360h\_progressive.webp)复制这个链接，粘贴浏览器打开![](https://i0.hdslb.com/bfs/article/357714be1061ac78c99a52d2a04500d7af498c94.png@942w\_651h\_progressive.webp)选择Quickstart![](https://i0.hdslb.com/bfs/article/d98c9adecd1e2b0513456309f8858b8bafe587dd.png@579w\_527h\_progressive.webp)允许之后，这就是code

![](<../.gitbook/assets/image (19).png>)

![](<../.gitbook/assets/image (20).png>)

![](<../.gitbook/assets/image (21).png>)

在你输入 code  权限开启后，回车之后还会有一个 链接 末显示“Press Enter to retry",这时，要留意前面的链接 普通英文翻译罢了 ，还是复制到浏览器打开 开启Service Usage API服务**,**开启你的**服务使用API**

![](<../.gitbook/assets/image (22).png>)

启用成功后 回车

你的cmd 就开始运行了,创建了一个项目

![](<../.gitbook/assets/image (23).png>)

接着 你就会发现你的Au­toR­clone 文件下面的 ac­counts 文件夹下会有很多的 json 文件。

大约是200个吧 为什么要弄一个项目呢？ 这里说一下随意多建项目的话 ，不仅耗时 而且多余的项目一个月之后才能删除 而1个项目 100个sa 一天75T 对于正常人足够用了！！！

## 提取sa邮箱

{% tabs %}
{% tab title="windows" %}
使用一个小工具，自动提取当前目录下json的邮箱

[https://github.com/cgkings/script-store/blob/master/tools/getuserByFeiFan.exe](https://github.com/cgkings/script-store/blob/master/tools/getuserByFeiFan.exe)
{% endtab %}

{% tab title="linux" %}
有命令也有脚本，但是时间长都忘了，尴尬
{% endtab %}
{% endtabs %}



## 普通账号可以去网上找到Team Drive &#x20;

这里给几个**免费**的吧

[https://gd.404edu.workers.dev/](https://gd.404edu.workers.dev)

[https://gd.zxd.workers.dev/](https://gd.zxd.workers.dev)

输入你的Google drive 的邮箱便可自动创建  **理论上是无限盘（切记 勿放重要资料）**

