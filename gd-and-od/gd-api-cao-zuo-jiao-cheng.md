# gd API操作教程

## **生成服务项目**

首先开启 Drive API （[https://developers.google.com/drive/api/v3/quickstart/python](https://developers.google.com/drive/api/v3/quickstart/python)）

![](<../.gitbook/assets/image (17).png>)

![](<../.gitbook/assets/image (18).png>)

然后将你下载的`credentials.json`放到AutoRclone目录下

## 管理SA

你可以在 Google API（[https://console.developers.google.com/apis/dashboard](https://console.developers.google.com/apis/dashboard)） 看到你的项目及 SA

点开全部 就能看到你all 的sa  (当然你也可以不用看)![](https://i0.hdslb.com/bfs/article/2a1c865866531d2544f038e4ddf2fadc223c2416.png@942w\_545h\_progressive.webp)

只是为了让你理解你的服务账号在哪

![](<../.gitbook/assets/image (24).png>)

接下来 你就发现 每一个项目都有100个电子邮件 ！对 就是他 \


接下来的任务就是把这个100个邮箱 复制 并记录下来..................................................![](https://i0.hdslb.com/bfs/article/02db465212d3c374a43c60fa2625cc1caeaab796.png@progressive.webp)

确实要是按照正常方式来说，似乎是一个天文数字.......

网上有很多这玩意的教程 例如 复制到Excel表格整理 安装Chrome 插件之类的都是好方法&#x20;

但我推荐你一个更好的办法,提取sa的bat,放到你的Au­toR­clone 文件下面的 ac­counts 文件夹 运行一下,他就会提取你的Json文件的邮箱  并自动生成一个txt![](https://i0.hdslb.com/bfs/article/af60301cbbd6ebee0ea6a19813bf05f258081621.png@942w\_531h\_progressive.webp)保存下来吧

接着 你需要把这些邮箱放进你的Group&#x20;

## 添加SA到群组

直接创建一个 Google Group （[https://groups.google.com/](https://groups.google.com)）然后手动地将 ser­vice ac­counts 对应的邮箱地址，挨个加进去。但每次只能加 10 个，一个群组每 24 小时只能加 100 个。![](https://i0.hdslb.com/bfs/article/89b3b84ecc0a01040fb7b606db03b090105c9e7c.png@942w\_531h\_progressive.webp)![](https://i0.hdslb.com/bfs/article/f4bdd7be69166cca75ee3d5eb061e869b0a188d0.png@942w\_531h\_progressive.webp)

接下来 你开始复制粘贴就行  记住 第一天只能添加100个邮箱&#x20;

之后![](https://i0.hdslb.com/bfs/article/285b726f6396bd9f046419d6034d41bfdf528f8a.png@942w\_531h\_progressive.webp),记住这个邮箱 就是刚刚你创建的邮箱 复制一下 ![](https://i0.hdslb.com/bfs/article/55fdacae83753cc12462c28fb9d9f728235ea3c9.png@942w\_531h\_progressive.webp)当然若你是G Suite 或者edu 也可以自己创建团队盘









