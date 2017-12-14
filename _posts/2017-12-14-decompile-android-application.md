---
layout: post
title:  "反编译安卓程序"
date:   2017-12-14
---

## apktool反编译apk为smali代码

apktool反编译app-debug.apk，生成的文件放到`apk.out`目录。

```
$apktool d app-debug.apk -o apk.out
I: Using Apktool 2.3.0 on app-debug.apk
I: Loading resource table...
I: Decoding AndroidManifest.xml with resources...
S: WARNING: Could not write to (/home/ubuntu/.local/share/apktool/framework), using /tmp instead...
S: Please be aware this is a volatile directory and frameworks could go missing, please utilize --frame-path if the default storage directory is unavailable
I: Loading resource table from file: /tmp/1.apk
I: Regular manifest package...
I: Decoding file-resources...
I: Decoding values */* XMLs...
I: Baksmaling classes.dex...
I: Copying assets and libs...
I: Copying unknown files...
I: Copying original files...
```

对文件进行修改以后，用apktool重新打包修改过的工程目录，生成的apk文件位于`./apk.out/dist/`目录中。


```
$ apktool b apk.out/
I: Using Apktool 2.3.0
I: Checking whether sources has changed...
I: Smaling smali folder into classes.dex...
I: Checking whether resources has changed...
I: Building resources...
S: WARNING: Could not write to (/home/ubuntu/.local/share/apktool/framework), using /tmp instead...
S: Please be aware this is a volatile directory and frameworks could go missing, please utilize --frame-path if the default storage directory is unavailable
I: Building apk file...
I: Copying unknown files/dir...
```



## dex2jar使用方法

把dex2jar添加到PATH中，比如我使用的工作目录为`/home/ubuntu/bin/dex2jar-2.0/`。

```
cd /home/ubuntu/bin/dex2jar-2.0/
chmod a+x *.sh
echo "export PATH=$PATH:/home/ubuntu/bin/dex2jar-2.0/" > ~/.bashrc
source ~/.bashrc
```

解压apk文件到temp目录

```
$ unzip app-debug.apk -d temp
Archive:  app-debug.apk
  inflating: temp/AndroidManifest.xml  
  inflating: temp/META-INF/CERT.RSA  
  inflating: temp/META-INF/CERT.SF   
  inflating: temp/META-INF/MANIFEST.MF  
  inflating: temp/classes.dex        
  inflating: temp/res/drawable-anydpi-v21/ic_launcher_background.xml  
 extracting: temp/res/drawable-hdpi-v4/ic_launcher_background.png  
 extracting: temp/res/drawable-ldpi-v4/ic_launcher_background.png  
 extracting: temp/res/drawable-mdpi-v4/ic_launcher_background.png  
  inflating: temp/res/drawable-v24/$ic_launcher_foreground__0.xml  
  inflating: temp/res/drawable-v24/ic_launcher_foreground.xml  
 extracting: temp/res/drawable-xhdpi-v4/ic_launcher_background.png  
 extracting: temp/res/drawable-xxhdpi-v4/ic_launcher_background.png  
 extracting: temp/res/drawable-xxxhdpi-v4/ic_launcher_background.png  
  inflating: temp/res/layout/activity_main.xml  
  inflating: temp/res/mipmap-anydpi-v26/ic_launcher.xml  
  inflating: temp/res/mipmap-anydpi-v26/ic_launcher_round.xml  
 extracting: temp/res/mipmap-hdpi-v4/ic_launcher.png  
 extracting: temp/res/mipmap-hdpi-v4/ic_launcher_round.png  
 extracting: temp/res/mipmap-mdpi-v4/ic_launcher.png  
 extracting: temp/res/mipmap-mdpi-v4/ic_launcher_round.png  
 extracting: temp/res/mipmap-xhdpi-v4/ic_launcher.png  
 extracting: temp/res/mipmap-xhdpi-v4/ic_launcher_round.png  
 extracting: temp/res/mipmap-xxhdpi-v4/ic_launcher.png  
 extracting: temp/res/mipmap-xxhdpi-v4/ic_launcher_round.png  
 extracting: temp/res/mipmap-xxxhdpi-v4/ic_launcher.png  
 extracting: temp/res/mipmap-xxxhdpi-v4/ic_launcher_round.png  
 extracting: temp/resources.arsc     
```

把生成的classes.dex转换成jar文件。

```
$ d2j-dex2jar.sh temp/classes.dex 
dex2jar temp/classes.dex -> ./classes-dex2jar.jar
```

然后使用`JD-GUI`打开jar文件即可。

## Reference

 - Android软件安全与逆向分析

