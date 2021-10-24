# debian设置root登录ssh权限

## 现有账户提权root

很简单的一条命令，输入后将需要你输入root密码即可提升为root账户：

```bash
su -
```

## 修改root登录项目

1、修改/etc/ssh/sshd\_config文件，增加 “PermitRootLogin yes“

```bash
nano /etc/ssh/sshd_config
```

2、重启ssh服务生效

```bash
service sshd restart
```

