---
layout: post
title:  "一加手机后门"
date:   2017-11-18
---

转自twitter用户fs0c131y


<Thread> Hey @OnePlus! I don't think this EngineerMode APK must be in an user build...
This app is a system app made by @Qualcomm and customised by @OnePlus. It's used by the operator in the factory to test the devices.


[](https://explorerlxz.github.io/images/oneplus-backdoor/DOhukUhX4AAeSxr.jpg)

If you have an OnePlus device, I'm pretty sure you have this app pre-installed. To check open Settings -> Apps -> Menu -> Show system apps and search EngineerMode in the app list to check

With telephony secret code you can access to manual tests like GPS test, root status test as stated in this article https://www.xda-developers.com/oneplus-hardware-diagnostic-tests … pointed by @AleGrechi . But can do better...

[](https://explorerlxz.github.io/images/oneplus-backdoor/DOhzttTW0AM90oN.jpg)

[](https://explorerlxz.github.io/images/oneplus-backdoor/DOh0DhoWkAAG85F.jpg)

You can access to the "main" activity by sending this command: adb shell am start http://com.android .engineeringmode/.EngineeringMode You will have access to everything, not just the manual test.


### 安卓多用户

我发现用户的数据存在`/data/media`目录下面，第一个用户的sdcard映射到`0`目录，后面的用户sdcard可能映射到`11`目录。

多用户模式下，不同用户不能安装同一个软件，也可能只有在recovery中安装才对所有用户有效吧。
