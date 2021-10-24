# 解决端口占用问题

## 1、查询端口占用程序

比如查询80端口占用

```
netstat -lnp|grep 80
```

{% hint style="info" %}
netstat如果未安装，则apt-get install net-tools
{% endhint %}

![](<../.gitbook/assets/image (74).png>)

## 2、结束占用端口的程序

比如上面查询到的占用端口的程序是httpd,其pid号是3135

`kill -9 3135`

结束掉后，再`netstat -lnp|grep 80`看看占用











kk
