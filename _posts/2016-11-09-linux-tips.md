---
layout: post
title:  "Linux使用心得"
date:   2016-11-09
---

## 挂載Virtualbox的共享文件夾

宿主機爲Windows 10，虛擬機爲Virtualbox，想要挂載VM_Share文件夾到Arch虛擬機xwr用戶下的Shared_files文件，並給此用戶全部讀寫權限。

這個目的既可以在命令行下實現，也可以在配置文件中永久實現。

在命令行下實現，需要root權限，且只能暫時使用，重啓之後失效

```
mount VM_Share /home/xwr/Shared_files -t vboxsf -o rw,gid=100,uid=1000
```

把配置信息寫入配置文件，則可以開機會自動挂載共享文件夾

在/etc/fstab文件中添加下面的配置信息

```
VM_Share /home/xwr/Shared_files vboxsf rw,gid=100,uid=1000,auto 0 0
```

重啓系統後以root身份執行

```
systemctl status home-xwr-Shared_files.mount 
```

得到下面的信息，說明文件挂載成功

```
● home-xwr-Shared_files.mount - /home/xwr/Shared_files
   Loaded: loaded (/etc/fstab; generated; vendor preset: disabled)
   Active: active (mounted) since Wed 2016-11-09 13:59:32 CST; 13min ago
    Where: /home/xwr/Shared_files
     What: VM_Share
     Docs: man:fstab(5)
           man:systemd-fstab-generator(8)
  Process: 164 ExecMount=/usr/bin/mount VM_Share /home/xwr/Shared_files -t vboxsf -o rw,gid=100,uid=1000 (code=exited, $
    Tasks: 0 (limit: 4915)
   CGroup: /system.slice/home-xwr-Shared_files.mount
```

## Resilio(BT Sync)

這個軟件原來叫做BT Sync，後來改名字成Resilio了，雖然改名字了，但是功能還是非常強大的，P2P加密分享文件，在網盤沒落，Torrent，magnet站點關停的情況下對我們來說是個福音，linux下可以用firefox進行同步，在瀏覽器中輸入http://localhost:8888/gui/再輸入帳號密碼即可，windows下直接運行就可以了，不需要瀏覽器，但是這個軟件在我的win 10中總會讓顯卡出問題，因此還是放在虛擬機里用吧！


## ntpd服務（更正系統時間）

今天把arch系統休眠了，結果恢復後時間沒有變回來，用date之類的命令都不管用，從網上找到一個解決方案

>>The correct way to do this would be by enabling ntpd.service via systemd.
>>
# pacman -Syu ntp Installed the required package
>>
# systemctl enable ntpd.service Enable it at boot so every time you boot the system the clock will be synchronized
>>
# systemctl start ntpd.service Start it immediately
>>
One could also run ntpd -qg as root.
>>
Once you have systemd managing this operation, you should never have to worry about setting the clock agian.

簡單總結一下

```
pacman -S ntp
systemctl enable ntpd.service
systemctl start ntpd.service
ntpd -qg
```

## 整理磁力鏈接

由於對Python瞭解不夠，對數據庫學習也不深入，就先不搞全自動爬蟲了。先來手工實現一下：

在某個Torrent站點找到自己想要的種子，挑選好後，Ctrl+S保存到本地。如index.htm。

用vi編輯器把網頁中所有的"替換成回車，在編輯器中輸入如下字串執行即可

```
:%s/"/^M/g
```

其中^通過按Ctrl-v,M通過按Ctrl-m即可出來，替換之後保存文件。

執行一下命令

```
cat index.htm | grep magnet: >> result.txt
```

這樣我們就把想要下載的東西保存到result.txt了。下載的話執行aria2c -i result.txt即可，非常方便。

以後有空用Python實現一下爬蟲，數據庫，有需要的話再整個硬盤。

## Reference

