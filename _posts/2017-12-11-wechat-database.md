---
layout: post
title:  "解密微信数据库"
date:   2017-12-11
---

首先需要一个root过的Android机。

找到微信聊天数据库`/data/data/com.tencent.mm/MicroMsg/xxxxxxxxxxxxxxxx/EnMicroMsg.db`，复制到电脑上。

解密方法，安装sqlcipher，获取手机IMEI以及微信的UIN，计算出`KEY=md5(IMEI+UIN).subString(0,7)`。

然后执行`sqlcipher EnMicroMsg.db`

```
PRAGMA key = KEY; # 这是key
PRAGMA cipher_use_hmac = off; # 禁用HMAC验证，以兼容sqlcipher 1.1.x数据库
PRAGMA cipher_page_size = 1024;
PRAGMA kdf_iter = 4000; # 迭代次数，需要指定。否则新版sqlcipher无法解密
```

![](https://explorerlxz.github.io/images/wechat-database/decrypte.png)


此方法在WeiXin-V6.5.22上仍然可用。


## Reference

 - CSDN:[Android逆向之旅---静态方式破解微信获取聊天记录和通讯录信息 ](http://blog.csdn.net/jiangwei0910410003/article/details/52238891)
 - [朋友圈小视频替换大法](https://lixingcong.github.io/2016/03/28/wechat-moment-video-reaplace/)
