# FQ搭建工具介绍

## 简介

留空

## 一、X-UI

XUI面板安装 新面板xui，新增流量限制时间限制，估计准备转型当机场面板了😂😂😂

bash <(curl -Ls https://raw.githubusercontent.com/sprov065/x-ui/master/install.sh)



```
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

如果是全新安装，默认网页端口为 54321，用户名和密码默认都是 admin 请自行确保此端口没有被其他程序占用，并且确保 54321 端口已放行

如果是更新面板，则按你之前的方式访问面板

### 二、x-ui 管理脚本使用方法:

x-ui - 显示管理菜单 (功能更多)&#x20;

x-ui start - 启动 x-ui 面板&#x20;

x-ui stop - 停止 x-ui 面板&#x20;

x-ui restart - 重启 x-ui 面板&#x20;

x-ui status - 查看 x-ui 状态&#x20;

x-ui enable - 设置 x-ui 开机自启&#x20;

x-ui disable - 取消 x-ui 开机自启&#x20;

x-ui log - 查看 x-ui 日志&#x20;

x-ui v2-ui - 迁移本机器的 v2-ui 账号数据至 x-ui&#x20;

x-ui update - 更新 x-ui 面板&#x20;

x-ui install - 安装 x-ui 面板&#x20;

x-ui uninstall - 卸载 x-ui 面板

## 三、x-ui进程守护脚本参考

可以及时守护x-ui和xray的进程，保证科学随时畅游

### 1.第一步【以下内容全部复制到连接服务器的终端里执行】

```
cat>/root/x_ui_proc.sh<<-\EOF
#!/bin/bash
x_ui1=ps -aux |grep "x-ui" |grep -v "grep" |wc -l
x_ui2=ps -aux |grep "xray" |grep -v "grep" |wc -l
sleep 0.5
if [ $x_ui1 = 0 ];then
x-ui restart
fi
if [ $x_ui2 = 0 ];then
x-ui restart
fi
EOF
```

\#如图 cat /root/x\_ui\_proc.sh 应该是这个样子：\
[![image](https://user-images.githubusercontent.com/59961153/127632874-7f410253-66e7-4883-9ae1-51a1196b572f.png)](https://user-images.githubusercontent.com/59961153/127632874-7f410253-66e7-4883-9ae1-51a1196b572f.png)

### 2.第二步

以下内容全部复制到连接服务器的终端里执行

```
chmod +x /root/x_ui_proc.sh
```

### 3.第三步【以下内容全部复制到连接服务器的终端里执行】

\#以下内容全部复制到连接服务器的终端里执行，每分启动一次检查

```
(crontab -l ; echo "*/1 * * * * bash /root/x_ui_proc.sh > /dev/null") | crontab - && crontab -l && /etc/init.d/cron restart
```

## 四、其他注意事项

1.服务器如果配置有防火墙，请记得放行相关的端口\
2.证书申请完毕后，打开x-ui后台-面板设置\
“面板证书公钥文件路径”里填写/root/.ssh/cert.crt\
“面板证书密钥文件路径”里填写/root/.ssh/private.key\
重启面板即可\
3.讨论群：[https://t.me/openwrt\_lede\_v2ray\_plugin](https://t.me/openwrt\_lede\_v2ray\_plugin)\
4.文中证书申请一节部分参考波仔博客，特别感谢！\
5.特别鸣谢sprov065大佬的辛苦付出！

\






