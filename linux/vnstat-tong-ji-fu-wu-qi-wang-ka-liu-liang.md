# vnstat 统计服务器网卡流量

## 安装说明

在 Debian/Ubuntu 下非常简单:

```
sudo apt install vnstat vnstati
```

&#x20;假设网卡名为 `eth0`，该配置在 `/etc/vnstat.conf` 中

安装结束，使用以下命令建立数据库， 数据库目录：`/var/lib/vnstat/`:

```bash
sudo vnstat -u -i eth0
```

添加为开机启动

```bash
sudo update-rc.d vnstat enable
```

## 使用说明

&#x20;直接输入 `vnstat`

```bash
vnstat -l  # 或者 `--live` 实时流量
vnstat -h  # 显示小时流量
vnstat -d  # 显示日流量信息
vnstat -w  # 显示周流量信息
vnstat -m  # 显示月流量信息
vnstat -t  # 显示流量最高top10天
```

&#x20;图形化输出可以使用 `vnstati` ，将月流量统计图输出到图片

```bash
vnstati -i eth0 - -months - -output /dir/month.png
```

## 删除数据库

```bash
vnstat --delete --force -i eth0
```

## 服务操作

启动 vn­Stat：`service vnstat start`

停止 vn­Stat：`service vnstat stop`

重启 vn­Stat：`service vnstat restart`

查看 vn­Stat 状态：`service vnstat status`
