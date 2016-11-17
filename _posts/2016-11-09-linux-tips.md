---
layout: post
title:  "Linux使用筆記"
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

>> The correct way to do this would be by enabling ntpd.service via systemd.
>>
**pacman -Syu ntp** Installed the required package
>>
**systemctl enable ntpd.service** Enable it at boot so every time you boot the system the clock will be synchronized
>>
**systemctl start ntpd.service** Start it immediately
>>
One could also run **ntpd -qg** as root.
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

## wget下載特定目錄

曾經有一次想要下載某一個網站2016年的信息安全方面的論文，直接用**wget -c http://eprint.iacr.org/2016/**，我以爲只會下載一個目錄，沒想到把整個網站都快下載下來了……

下載特定目錄可以用這個命令

```
wget -c -r -np -k -L -p www.xxx.org/pub/path/

-c 断点续传
-r 递归下载，下载指定网页某一目录下（包括子目录）的所有文件
-nd 递归下载时不创建一层一层的目录，把所有的文件下载到当前目录
-np 递归下载时不搜索上层目录，如wget -c -r www.xxx.org/pub/path/
没有加参数-np，就会同时下载path的上一级目录pub下的其它文件
-k 将绝对链接转为相对链接，下载整个站点后脱机浏览网页，最好加上这个参数
-L 递归时不进入其它主机，如wget -c -r www.xxx.org/ 
如果网站内有一个这样的链接： 
www.yyy.org，不加参数-L，就会像大火烧山一样，会递归下载www.yyy.org网站
-p 下载网页所需的所有文件，如图片等
-A 指定要下载的文件样式列表，多个样式用逗号分隔
-i 后面跟一个文件，文件内指明要下载的URL
```

更多參考：[wget 下载整个网站，或者特定目录](http://www.cnblogs.com/lidp/archive/2010/03/02/1696447.html)

註：今天嘗試下載王垠的博客[當然我在扯淡](http://www.yinwang.org)時只下載了三五篇就結束了，不知道怎麼回事兒，也許是他網站做了防爬蟲吧，以後再試試-2016-11-17

## python程序從文本中提取含有中文的字符串/行

下面是從博客園中搬運過來的，鏈接爲[Python-提取文件中所有中文小程序](http://www.cnblogs.com/xinzaitian/archive/2010/11/30/1892208.html)。

```Python
#coding=utf-8 
import imp 
import sys 
imp.reload(sys) 
sys.setdefaultencoding('utf-8') #设置默认编码,只能是utf-8,下面\u4e00-\u9fa5要求的
import re   
pchinese=re.compile('([\u4e00-\u9fa5]+)+?') #判断是否为中文的正则表达式
f=open("data.txt") #打开要提取的文件
fw=open("getdata.txt","w")#打开要写入的文件
for line in f.readlines():   #循环读取要读取文件的每一行
    m=pchinese.findall(str(line)) #使用正则表达获取中文
    if m:
        str1='|'.join(m)#同行的中文用竖杠区分
        str2=str(str1)
        fw.write(str2)#写入文件
        fw.write("\n")#不同行的要换行
f.close()
fw.close()#打开的文件记得关闭哦!
```
在我的系統上提示第五行錯誤，sys中沒有setdefaultencoding函數，我直接把這一行註釋掉了，結果正常運行。

我的需求是提取帶有中文字符串的所有行，由於我最近在破解一個VB程序，用VB Decompiler獲取到反編譯出來的代碼(行首有定位信息)，查找突破口，但是代碼量太大了，我也不清楚錯誤提示是什麼，但知道肯定是中文。由於不動地如何用grep匹配帶有中文的行，只好用Python了！能解決問題才是關鍵。

註：這個VB程序我不能用OD或者IDA查看Disassemble出來的代碼，程序識別不出來，全當成數據了，也不能直接運行，需要加密狗什麼玩意兒。


```Python
#coding=utf-8 
import imp 
import sys 
imp.reload(sys) 
#sys.setdefaultencoding('utf-8') #设置默认编码,只能是utf-8,下面\u4e00-\u9fa5要求的
import re   
pchinese=re.compile('([\u4e00-\u9fa5]+)+?') #判断是否为中文的正则表达式
f=open("data.txt") #打开要提取的文件
fw=open("getdata.txt","w")#打开要写入的文件
for line in f.readlines():   #循环读取要读取文件的每一行
    m=pchinese.findall(str(line)) #使用正则表达获取中文
    if m:
#        str1='|'.join(m)#同行的中文用竖杠区分
#        str2=str(str1)
#        fw.write(str2)#写入文件
        fw.write(line)
#        fw.write("\n")#不同行的要换行
f.close()
fw.close()#打开的文件记得关闭哦!

```



## Reference

 - 博客園：[wget 下载整个网站，或者特定目录](http://www.cnblogs.com/lidp/archive/2010/03/02/1696447.html)
 - 博客園：[Python-提取文件中所有中文小程序](http://www.cnblogs.com/xinzaitian/archive/2010/11/30/1892208.html)
