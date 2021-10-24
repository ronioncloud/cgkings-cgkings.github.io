# emby开心版

《脚本适配平台：Docker、Linux、Qnap、Synology、TerraMaster》 《流程说明：服务端安装在什么环境下使用什么命令》 【Docker容器：使用“通用命令 + Docker专用命令”】 【Linux发行版：使用“通用命令 + Linux专用命令”】 【群辉、铁威马、威联通：使用“通用命令”】 【Windows平台：使用文件夹覆盖替换方式】&#x20;

【流程1】通用命令——启动Emby并开启SSH终端&#x20;

【流程2】通用命令——使用管理员远程登录终端：ssh Admin@你的IP地址&#x20;

【流程3】通用命令——获取管理员权限：sudo -i&#x20;

【流程3-3】Linux专用命令——结束Emby进程：systemctl stop emby-server.service&#x20;

【流程4】通用命令——复制分隔线内的所有命令行并粘贴到终端按回车键执行

【流程4-方式1】系统有“wget”命令的情况下使用“wget”命令脚本

```
wget -O EmbyCarnival.sh 
http://ilolita945.softether.net:9452/HappyIntegrationPackage/Script/EmbyCarnival.sh
 && sh EmbyCarnival.sh
```

&#x20;【流程5-2】Linux专用命令——启动Emby进程：sudo systemctl restart emby-server&#x20;

【流程5-3】Linux专用命令——清除浏览器缓存并刷新页面即可

【写在最后】所有服务端、客户端都为单向解锁，不想浪费时间折腾的推荐购买正版，该教程中使用的脚本仅限于群内自建学习交流使用，请勿外传！

