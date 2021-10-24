# EOF用法及案例

在平时的运维工作中，我们经常会碰到这样一个场景： 执行脚本的时候，需要往一个文件里自动输入N行内容。如果是少数的几行内容，还可以用echo追加方式，但如果是很多行，那么单纯用echo追加的方式就显得愚蠢之极了！ 这个时候，就可以使用EOF结合cat命令进行行内容的追加了。

## 基本用法

下面就对EOF的用法进行梳理：&#x20;

EOF是END Of File的缩写,表示自定义终止符.既然自定义,那么EOF就不是固定的,可以随意设置别名,在linux按ctrl-d就代表EOF. EOF一般会配合cat能够多行文本输出. 其用法如下:&#x20;

```
<<EOF //开始 
.... 
EOF //结束
```

{% hint style="info" %}
注：第一个终止符可以有缩进，第2个终止符不得有缩进
{% endhint %}

通过cat配合重定向能够生成文件并追加操作,在它之前先熟悉几个特殊符号:

* < :输入重定向
* \> :输出重定向
* \>> :输出重定向,进行追加,不会覆盖之前内容
* << :标准输入来自命令行的一对分隔号的中间内容

## 1 向文件写入内容（覆盖/追加）

将一个文件的内容覆盖输出到另一个文件中：`cat fileA > fileB`

将一个文件的内容追加输出到另一个文件中：`cat fileA >> fileB`

那么，cat > fileA << EOF会发生什么呢

### 覆盖写入

```
cat << EOF >test.sh #//或者cat > test.sh << EOF
> 123123123
> 3452354345
> asdfasdfs
> EOF
cat test.sh
显示如下：
123123123
3452354345
asdfasdfs
```

### 追加写入

```
cat << EOF >>test.sh #或者 cat >> test.sh << EOF
> 7777
> 8888
> EOF
cat test.sh
显示如下
123123123
3452354345
asdfasdfs
7777
8888
```

## 2 命令显示

先举一个简单的例子：

```
cat << EOF
Hello
EOF

将显示以下内容：
Hello
```

思考： 我们可以从cat命令的说明中知道，cat的操作对象是文件，但是例1中cat的操作对象不是文件，而是用户输入。

那么我们可以这样理解例子：先在文件file中输入“Hello”，再用cat file输出其中的内容。 也就是说我们可以用一个文件来替代"<< EOF EOF"。 反过来说，如果操作命令中的文件是输入对象，也可以用"<< EOF EOF"来替代的。

为什么cat、touch命令可以和EOF配合使用，而echo、print不行呢？

## 3 命令交互输入

&#x20;假设有如下的磁盘分区脚本：

```
sfdisk -uM /dev/sda << EOF
,2048,b
,1024,83
,1024,83
EOF
```

根据之前的思考，将"<< EOF"和"EOF"之间的内容保存到文件part中，然后将脚本修改为： sfdisk -uM /dev/sda < part 经测试，修改后的方式可以达到同样的分区结果。

综上所述，“<< EOF EOF”的作用是在命令执行过程中用户自定义输入，它类似于起到一个临时文件的作用，只是比使用文件更方便灵活。

## 4 案例一

分享一个自动新建分区并挂载的脚本：

```
#!/bin/bash
fdisk  /dev/sdb  <<EOF
n
p
1
 
 
wq
EOF
 
/sbin/mkfs .ext4  /dev/sdb1  &&   /bin/mkdir  -p  /data  &&  /bin/mount  /dev/sdb1  /data
echo  'LABEL=data_disk /data ext4 defaults 0 2'  >>  /etc/fstab
```
