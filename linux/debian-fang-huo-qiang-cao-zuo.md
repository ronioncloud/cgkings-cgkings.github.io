# debian防火墙操作

## 1、放行端口

防火墙放行 8070 端口:

```bash
# 放行8070端口
sudo iptables -I INPUT -p tcp --dport 8070 -j ACCEPT
# 保存设置
sudo iptables-save
```

{% hint style="info" %}
&#x20;Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}

