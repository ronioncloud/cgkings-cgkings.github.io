# gd to od

#### 1、微软OneDrive 单账号/全域/api相关动态 限额策略简析 <a href="1-wei-ruan-onedrive-dan-zhang-hao-quan-yu-api-xiang-guan-dong-tai-xianece-lve-jian-xi-1" id="1-wei-ruan-onedrive-dan-zhang-hao-quan-yu-api-xiang-guan-dong-tai-xianece-lve-jian-xi-1"></a>

1.单账号稳定1Gbps传输速度内，2tb后就会限速，如果超速，1tb就限制你也是正常的\
2.微软的限速策略是动态智能限制，处于触发状态后，你如果进行类似高负荷操作，很快就会再进宫\
3.即便你隔了3，5日后再操作，依然会很快被标记\
4.根据经验，50-70MB/s的速度可以持续跑到2tb，无论多慢的平均速度，依然注意存在2tb左右的额度触发\
5.单api有线程限制，推荐不要超过16线程，超过会容易提前触发限制\
6.单账号下申请多个私有api也有一个共享的额度和线程限制，长期大量拷贝不推荐单账号下申请多个独立api，而是推荐域内每一个账号申请对应1个api即可\
7.总域仍然有总线程数和平均速度以及额度限制，所有账号对应的api，总线程不推荐超过40线程，32-36线程比较稳定可以持续跑，但是注意依然有总额度10tb左右额度限制触发\
8.微软的服务器有物理限速瓶颈，总带宽大约是3Gbps-4Gbps左右，如果你的带宽超过此国际带宽总额，似乎没有很好的办法提升\
9.微软的存储架构存在吞吐量的不足，长时间大量拷贝，大约200tb左右会频繁触发service unavailable 错误，导致文件无法成功传输，推测可能是硬盘io问题，或者数据库结构瓶颈，请等待机房为你个人增加硬盘，系统恢复后再拷贝\
10.结合第二点，推荐准备至少10个账号和对应的10个api，这样可以使得你在微软的多维规则限制下，瓶颈不在于你自己，分为两组，交替使用，以免“过热”\
11.微软的下载取回速度是远低于存入的，请做好翻车的心理准备，1个月你能取回的数据可能只有50tb左右，如果是正规车要翻车，1个月的时间，也拯救不了你的。

#### 2、Rclone拷贝到OneDrive的参数配置简介 <a href="2rclone-kao-bei-dao-onedrive-de-can-shu-pei-zhi-jian-jie-3" id="2rclone-kao-bei-dao-onedrive-de-can-shu-pei-zhi-jian-jie-3"></a>

关于速度解释上面基本上没多少出入，自己根据手头资源调整就行\
一般的buyvm开4线程，8线程足够了\
开下swap，对于稳定持续速度很重要, 512M内存开512M swap即可[【转】CentOS系统增加swap交换空间文章转自# 关于CentOS7下swap分区创建(添加),删除以及相关配置在添加swap分区之前我们可以了解下当前...](https://omo.moe/archives/738/)

如果文档和破解软件多，再加上这些：

```
--ignore-checksum --ignore-size --drive-acknowledge-abuse  --retries 1 --log-file=/root/error.txt
```

最后把log里面的错误文档补一次就好，基本上就是病毒软件或者乱码文件，会拷不动\
buyvm 1G内存款，rclone日常拷贝参考此参数flag配置：

```
--transfers 8  --cache-chunk-size 20M  --bwlimit 120M --max-size 100G
```

基本上512m就4线程，1gb的话6-8线程足够。

`--cache-chunk-size`很重要，入门配置中不要超过20M，5M，10M其实足够的\
一些参数和环境配置的解释和参考，包括对于googledrive历史遗留旧文件永久无法下载的403bug文件的处理方法：

