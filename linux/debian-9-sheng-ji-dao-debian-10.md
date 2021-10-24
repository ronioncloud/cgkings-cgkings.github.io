# debian 9升级到debian 10

## step1:**更新**

```
apt-get update && apt-get upgrade
```

## **step2:备份一份**

```
cp /etc/apt/sources.list /etc/apt/sources.list.orig
```

## **step3:将/etc/apt/sources.list里所有“stretch”替换为“buster”**

```
sed -i 's/stretch/buster/g' /etc/apt/sources.list
```

## **step4:再更新一下**

```
apt-get update && apt-get upgrade
```

## **step5:执行升级命令**

```
apt-get dist-upgrade
```

## **step6:提示完成后重启**

```
reboot
```

## **step7:查看下当前debian版本**

```
lsb_release -a
```
