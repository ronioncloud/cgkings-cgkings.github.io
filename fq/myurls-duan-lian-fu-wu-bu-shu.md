---
description: ⚠️本教程基于掌握基础的服务器操作与代码修改技能之上
---

# MyUrls 短链服务部署

​[MyUrls](https://github.com/CareyWang/MyUrls) 是基于 golang1.13 与 Redis 实现的本地短链接服务，用于缩短请求链接与短链接还原

![](https://agwa5783.gitbook.io/\~/files/v0/b/gitbook-28427.appspot.com/o/assets%2F-M1inSzG-42ledaB5Umy%2F-M3tn2Lqc1JFZqkMMyXe%2F-M3tnpxSjljlrSDhzwiU%2FMyUrls.png?alt=media\&token=a11ef19a-70cd-4fb4-af54-37c1f444d3b3)

## 前期准备：

1、一台 VPS，普通配置即可。

2、一个域名，用于访问服务。（请提前使用 Cloudflare 做好域名解析以备用）

⚠️ 本文以 suo.yt 代表短链服务访问域名，牵扯到域名的操作请自行将相关参数修改为自己的域名。

⚠️ 请 Fork Sub-Web 前端源码后修改 /src/views/Subconverter.vue 文件中第 223 行「const shortUrlBackend = "https://suo.yt/short";」并保存。

⚠️ 建议开启 Cloudflare 的 CDN，避免 VPS 的 IP 被攻击。

⚠️ 部署完成后域名 SSL/TLS 加密模式必须为 Full，否则前端跨域配置可能会失效。

## 1、Dependencies <a href="#1-dependencies" id="1-dependencies"></a>

这里以 Ubuntu 18 系统为例，其它系统所使用的安装命令请自行研究：

执行以下命令安装 Redis 服务：

```
sudo apt-get update
sudo apt-get install redis-server -y
```

{% hint style="info" %}
本服务依赖于 Redis 提供长短链接映射关系存储，你需要本地安装 Redis 服务来保证短链接服务的正常运行。
{% endhint %}

## 2、安装 PM2 <a href="#2-an-zhuang-pm2" id="2-an-zhuang-pm2"></a>

执行以下命令安装 PM2 环境：

```
npm install -g pm2
```

## 3、下载并运行 MyUrls <a href="#3-xia-zai-bing-yun-hang-myurls" id="3-xia-zai-bing-yun-hang-myurls"></a>

执行以下命令以开启服务进程：

```
cd /home && wget https://github.com/CareyWang/MyUrls/releases/download/v1.10/linux-amd64.tar.gz
tar -zxvf linux-amd64.tar.gz && cd myurls
pm2 start linux-amd64-myurls.service --watch --name 大锤短链接 --max-memory-restart 50M -- -domain x-go.gq -port 8001
```

{% hint style="info" %}
以上命令中 suo.yt 域名需在自行部署服务时更换为自己的域名
{% endhint %}

## 4、修改前端 index.html <a href="#4-xiu-gai-qian-duan-index.html" id="4-xiu-gai-qian-duan-index.html"></a>

执行以下命令以编辑 index.html：

```
nano myurls/public/index.html
```

nano myurls/public/index.htmlCopied!

按下图所示，修改标记点为自己的域名后保存：

![](https://agwa5783.gitbook.io/\~/files/v0/b/gitbook-28427.appspot.com/o/assets%2F-M1inSzG-42ledaB5Umy%2F-M3zHy5oyTyTqGxhTFaV%2F-M3zIE7S34jdW4-a1Anw%2F%E7%A4%BA%E4%BE%8B.JPG?alt=media\&token=28b13efa-ee8f-4369-996f-f3b29a9ad5eb)

## 5、安装 Caddy2 <a href="#5-an-zhuang-caddy" id="5-an-zhuang-caddy"></a>

```
bash <(curl -sL git.io/cg_caddy2)
```

## 6、配置跨域 <a href="#6-pei-zhi-kua-yu" id="6-pei-zhi-kua-yu"></a>

复制粘贴以下全部命令到SSH执行：

```
cat << "EOF" >> /etc/caddy/Caddyfile
x-go.gq {
	file_server
	reverse_proxy localhost:8001
}
EOF
```

{% hint style="info" %}
将 x-go.gq 替换为自己申请的短链服务访问域名，此配置将会自动续签SSL证书。
{% endhint %}

## 7、重启 Caddy <a href="#7-zhong-qi-caddy" id="7-zhong-qi-caddy"></a>

执行以下命令以重启Caddy转发服务：

```
systemctl reload caddy
```

## 8、查看 Caddy 日志 <a href="#8-cha-kan-caddy-ri-zhi" id="8-cha-kan-caddy-ri-zhi"></a>

执行以下命令以查看日志，如果出现自己的域名，说明转发就成功了：

1tail -f /tmp/caddy.logCopied!至此短链转换服务 MyUrls 部署成功，你可在前端页面里生成并使用
