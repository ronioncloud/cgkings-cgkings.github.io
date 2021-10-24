# 安装qBittorrent增强版

## 1、安装步骤

发布地址：[https://software.opensuse.org//download.html?project=home%3Anikoneko%3Atest\&package=qbittorrent-enhanced](https://software.opensuse.org/download.html?project=home%3Anikoneko%3Atest\&package=qbittorrent-enhanced)

```bash
sudo apt-get update
wget https://download.opensuse.org/repositories/home:/nikoneko:/test/Debian_10/amd64/qbittorrent-enhanced_4.3.5.10-0+1.1_amd64.deb
apt install qbittorrent-enhanced_4.3.5.10-0+1.1_amd64.deb
apt-get install -y qbittorrent-nox
```

## 2、运行命令

```
qbittorrent-nox --webui-port=8070 -d
```

{% hint style="info" %}
&#x20;设定端口为8181，-d表示后台运行
{% endhint %}

## 3、设置服务，开机自启

3.1、创建qbittorrent-nox.service服务文件：

```bash
nano /etc/systemd/system/qbittorrent-nox.service
```

{% code title="qbittorrent-nox.service" %}
```bash
[Unit]
Description=qBittorrent-nox
After=network.target
[Service]
User=root
Type=forking
RemainAfterExit=yes
ExecStart=/usr/bin/qbittorrent-nox --webui-port=8070 -d
[Install]
WantedBy=multi-user.target
```
{% endcode %}

3.2、

重载并启动

```
sudo systemctl daemon-reload && sudo systemctl start qbittorrent-nox
```

开机自启

```
sudo systemctl enable qbittorrent-nox
```

## 4、使用配置

&#x20;默认用户名为 **admin** 密码为 **adminadmin**

**1、修改用户名密码**

**2、**
