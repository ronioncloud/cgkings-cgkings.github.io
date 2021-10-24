# systemctl教程



1 . Restart字段：定义了service退出后，Systemd 的重启方式

no（默认值）：退出后不会重启

on-success：只有正常退出时（退出状态码为0），才会重启

on-failure：非正常退出时（退出状态码非0），包括被信号终止和超时，才会重启

on-abnormal：只有被信号终止和超时，才会重启

on-abort：只有在收到没有捕捉到的信号终止时，才会重启

on-watchdog：超时退出，才会重启

always：不管是什么退出原因，总是重启

2 . RestartSec字段：表示 Systemd 重启服务之前，需要等待的秒数

3 . 修改配置文件以后，需要重新加载配置文件，然后重新启动相关服务

```
 systemctl daemon-reload
```

4 . Type字段

```
 Type=simple（默认值）：systemd认为该服务将立即启动。服务进程不会fork。如果该服务要启动其他服务，不要使用此类型启动，除非该服务是socket激活型。

 Type=forking：systemd认为当该服务进程fork，且父进程退出后服务启动成功。对于常规的守护进程（daemon），除非你确定此启动方式无法满足需求，使用此类型启动即可。使用此启动类型应同时指定 PIDFile=，以便systemd能够跟踪服务的主进程。
```
