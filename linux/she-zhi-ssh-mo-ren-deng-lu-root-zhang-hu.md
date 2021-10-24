# 设置ssh默认登录root账户

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

