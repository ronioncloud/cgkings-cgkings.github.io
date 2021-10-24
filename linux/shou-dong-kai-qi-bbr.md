# 手动开启BBR



**说明：**`Google BBR`刚出来的时候，一键包都还没出来，都是手动把内核换到`4.9`以上，从而开启`BBR`加速，方法可参考：[使用Google BBR拥塞算法加速TCP教程](https://www.moerats.com/archives/10/)，由于 `Debian 9`默认的就是`4.9`的内核而且编译了`TCP BBR`的内容，所以可以直接通过参数开启。

提示：目前最新版Debian 10内核为4.19，也可以直接用该方法开启BBR

## STEP1:手动开启

```
echo net.core.default_qdisc=fq >> /etc/sysctl.conf && echo net.ipv4.tcp_congestion_control=bbr >> /etc/sysctl.conf && sysctl -p
```

## STEP2：检查

{% tabs %}
{% tab title="方法一" %}
```
sysctl net.ipv4.tcp_available_congestion_control
```

显示以下即启动成功：

> sysctl net.ipv4.tcp\_available\_congestion\_control\
> net.ipv4.tcp\_available\_congestion\_control = bbr cubic reno
{% endtab %}

{% tab title="方法二" %}
```
lsmod | grep bbr
```

显示以下即启动成功：

lsmod | grep bbr

tcp\_bbr 20480 14
{% endtab %}
{% endtabs %}
