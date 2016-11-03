---
layout: post
title:  "树莓派"
date:   2016-10-29
---

昨天买的树莓派3B就要送到了，也买了个64G的闪迪TF卡用来装系统。本来想着随便玩玩，体验下win 10和kali，没想到从网上发现NAS这个玩意儿。挺有意思的，我也打算给家里做一个，希望可以远程控制。

号称Class 10，传输速度可达80M的闪迪TF卡并没有之前16G的卡传输速度快，反而更慢一些，今天解决了ftp中文乱码的问题，本以为应该在服务器端解决，结果发现并非如此，服务器需要支持中文，才可以在终端显示中文，但是传输数据编码还是要看客户端如何理解的，这里吧Filezilla客户端强制使用UTF-8字符集，就把这个问题解决了。(11月2日更)

今天在树莓派3B上体验了一下kali，设置静态ip老是出问题，刚开始时系统只利用了14.9G空间中的6.8G，后面的都没用，从网上找链接发现可以直接用fdisk调整空间大小

```
fdisk /dev/mmcblk0             #下面会显示两个分区
p							#显示分区
d							#删除后面的分区
n							#新建分区，并且设置范围（需要极其小心）

w							#写入磁盘
```

之后重启系统，过上一两分钟df -h命令查看一下，磁盘已经全部可以被利用了







## Reference

知乎：[自己家里搭建NAS服务器有什么好方案？](https://www.zhihu.com/question/21359049)
加州旅客's Blog:[用树莓派打造多功能家庭服务器](http://www.jiazhoulvke.com/2013/11/26/e794a8e6a091e88e93e6b4bee68993e980a0e5a49ae58a9fe883bde5aeb6e5baade69c8de58aa1e599a8/)
[如何解決 FileZilla 傳中文檔名亂碼問題](https://twnoc.net/support/Knowledgebase/Article/View/143/0/filezilla)
[RPi Resize Flash Partitions](http://elinux.org/RPi_Resize_Flash_Partitions)