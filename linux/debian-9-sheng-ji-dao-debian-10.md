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
**升级当前系统中的软件包**\


```
apt update && apt upgrade && apt full-upgrade
```

\
**更新 /etc/apt/sources.list 文件**\


```
cp /etc/apt/sources.list /etc/apt/sources.list.buster && \
cat << "EOF" > /etc/apt/sources.list
deb http://deb.debian.org/debian bullseye main contrib non-free
deb http://deb.debian.org/debian bullseye-updates main contrib non-free
deb http://security.debian.org/debian-security bullseye-security main
deb http://ftp.debian.org/debian bullseye-backports main contrib non-free
EOF
```



```
cp /etc/apt/sources.list /etc/apt/sources.list.buster && \
cat << "EOF" > /etc/apt/sources.list
deb http://mirrors.aliyun.com/debian-security bullseye-security main
deb http://mirrors.aliyun.com/debian bullseye main contrib non-free
deb http://mirrors.aliyun.com/debian bullseye-updates main contrib non-free
deb http://mirrors.aliyun.com/debian bullseye-backports main contrib non-free
EOF
```

\
**更新一次系统仓库列表，开始升级**\
\
**升级过程需要有人值守：因为部分软件的配置文件会出现变化，需要手动确认使用哪个版本。**\
\
**切记这个过程中不要中断，否则可能导致包关系异常或包管理系统损坏。**

```
apt update && apt upgrade && apt dist-upgrade && apt full-upgrade
```

**使用新的内核 image重启系统**

```
systemctl reboot
```

**查看系统版本**

```
cat /etc/os-release
```



> PRETTY\_NAME="Debian GNU/Linux 11 (bullseye)"\
> NAME="Debian GNU/Linux"\
> VERSION\_ID="11"\
> VERSION="11 (bullseye)"\
> VERSION\_CODENAME=bullseye\
> ID=debian\
> HOME\_URL="https://www.debian.org/"\
> SUPPORT\_URL="https://www.debian.org/support"\
> BUG\_REPORT\_URL="https://bugs.debian.org/"

**清理软件包**

```
apt autoremove && apt autoclean
```

```
dpkg --list | grep "^rc" | cut -d " " -f 3 | xargs dpkg --purge
```

**删除过时镜像**

```
dpkg --get-selections | grep linux
```

> linux-image-4.19.0-17-amd64                     install\
> linux-image-5.10.0-8-amd64                      install

```
apt autoremove --purge linux-image-4.19.0-17-amd64
```
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
