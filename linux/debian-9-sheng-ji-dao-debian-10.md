# 系统升级及DD重装

## 一、Debian升级

{% tabs %}
{% tab title="Debian 9 升 10" %}
### step1:**更新**

```
apt-get update && apt-get upgrade
```

### **step2:备份一份**

```
cp /etc/apt/sources.list /etc/apt/sources.list.orig
```

### **step3:将/etc/apt/sources.list里所有“stretch”替换为“buster”**

```
sed -i 's/stretch/buster/g' /etc/apt/sources.list
```

### **step4:再更新一下**

```
apt-get update && apt-get upgrade
```

### **step5:执行升级命令**

```
apt-get dist-upgrade
```

### **step6:提示完成后重启**

```
reboot
```

### **step7:查看下当前debian版本**

```
lsb_release -a
```


{% endtab %}

{% tab title="Debian 10 升 11" %}

{% endtab %}
{% endtabs %}



## 二、DD脚本

{% hint style="info" %}
DD有风险，操作需谨慎，后果是有可能VPS失恋，有可能会被删机器，有可能会被删号，不过后2种后果还没听说实际谁遇到过。

注意：要DD,先确认创建实例的镜像是Ubuntu 16或18，其它镜像能不能DD成功没试过！
{% endhint %}

为什么要DD？甲骨文的OS不是纯净系统，`rpcbind`监听，`oracle-cloud-agent`后台监控，系统firewall防护墙限制，系统端口限制，Ubuntu镜像默认设置了Iptable规则，以上种种全部要操作，直接DD最省心！

### step1：安装所需软件

{% tabs %}
{% tab title="debian/ubuntu" %}
```
sudo -i && apt-get install -y xz-utils openssl gawk file wget curl && apt-get update
```
{% endtab %}

{% tab title="centos" %}
```
yum install -y xz openssl gawk file wget curl && yum update
```
{% endtab %}
{% endtabs %}

### step2:运行萌咖的DD一键脚本

{% tabs %}
{% tab title="debian 9 10 11" %}
运行如下命令执行脚本：&#x20;

```
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -d 11 -v 64 -a -firmware -p "自定义root密码"
```

{% hint style="info" %}
注意：全自动安装，断连后稍等一段时间，使用如下默认用户密码登录ssh

默认root密码:MoeClub.org，可安装完成后passwd命令自行修改

也可以脚本命令中增加参数**`-p "自定义root密码" -port "自定义ssh端口"`**
{% endhint %}
{% endtab %}

{% tab title="ubuntu" %}
留空
{% endtab %}

{% tab title="windows" %}
留空
{% endtab %}
{% endtabs %}

![](<../.gitbook/assets/image (83).png>)

自动断开后，等会就可以使用新密码登录了！

OVER
