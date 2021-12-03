# ssh登录设置123

## 现有账户提权root

```
su -
```

{% hint style="info" %}
&#x20;将提示你输入root密码，即可转换为root账户
{% endhint %}

## 修改ssh默认登录账户

1、编辑/etc/ssh/sshd\_config文件，增加命令行"PermitRootLogin yes"

```bash
nano PermitRootLogin yes
```

2、重启ssh服务生效

```bash
service sshd restart
```

### **登陆之后的操作**

切换到root角色

```
sudo -i
```

#### **1、安装相关依赖**

CentOS：

```
yum -y install wget
yum update -y && yum install curl -y
```

Ubuntu：

```
apt-get install wget
apt-get update -y && apt-get install curl -y
```

#### **2、设置root密码**

```
passwd   # 修改密码
```

#### **3、开启SSH登陆**

```
sudo -i
vi /etc/ssh/sshd_config
```

#### **centOS**

按`i`进如编辑:

1、`PermitRootLogin`改为`yes`

![](https://pic4.zhimg.com/80/v2-e5f531e8ac26301689994fff0c70c863\_720w.jpg)

2、`PasswordAuthentication`改为`yes`

![](https://pic1.zhimg.com/80/v2-941244a9aa4a31ac6cf1dcc76a633de8\_720w.jpg)

`esc`,`:wq`,保存退出。

#### **Ubuntu**

ubuntu感觉是故意的，颜色搞成这样看也看不清楚……

可以最前面的`#`注释了，颜色会变亮一些，修改的部分如下：

![](https://pic1.zhimg.com/80/v2-3b226a2c6811887894e0e1affa3ac46c\_720w.jpg)![](https://pic2.zhimg.com/80/v2-46f95bc2da44a9832be9df9bd4a44ddd\_720w.jpg)

3、重启sshd服务

```
systemctl restart sshd.service
```

或者

```
sudo service sshd restart
```

好了，至此，我们可以愉快地像登陆普通VPS一样登陆甲骨文啦，换个终端登陆试试吧。

甲骨文云之所以这么做，其实是为了安全考量，用密钥登陆，取消root登陆，取消SSH登陆，会在一定程度上让我们的机子更安全，大家如果对自己的服务器安全性能要求高，也可以反向操作！
