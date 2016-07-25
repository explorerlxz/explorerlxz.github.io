---
layout: post
title: "选购笔记本，台式机，工作站"
date: 2016-07-25
---

经常有熟人问我应该怎么选购笔记本。唉，其实我真心不懂呀！毕竟我家买过的三台电脑，都是在同一个店里买的，网购实体店各种坑，我也不敢保证自己可以避免。我只是能够比较充分地利用已有的资源，被偷走的那台惠普450就被我玩的很溜，当初那台电脑报价4500，因为是老客户了，最好便宜到4050，又送了个耳机。

最近又寻思着整台电脑，台式机，服务器，工作站，组装机，还是自己攒机，一时也拿不定主意。我很倾向于攒台工作站，锻炼自己的动手能力，个性化配置，但也怕遇到难以解决的麻烦。

之前丢掉的惠普450笔记本，CPU应该是酷睿i5 3230M，今天CPU在Intel官网建议零售价为225.00$，笔记本在ZOL报价为3500左右。不过笔记本早已停产了。

等再调研两周再说吧。

公司目前给我配的电脑是**联想 启天B4550-B031(G1840)台式机**，CPU是*Intel(R) Celeron(R) CPU G1840 @ 2.80GHz*，今天在Intel官网建议零售价为42.00$。

![Lenovo B4550-B031](https://explorerlxz.github.io/images/Lenovo-celeron.png)

linux下查看了下CPU的配置

```
ubuntu@studio:~$ cat /proc/cpuinfo
processor   : 0
vendor_id   : GenuineIntel
cpu family  : 6
model       : 60
model name  : Intel(R) Celeron(R) CPU G1840 @ 2.80GHz
stepping    : 3
microcode   : 0x1d
cpu MHz     : 2800.000
cache size  : 2048 KB
physical id : 0
siblings    : 2
core id     : 0
cpu cores   : 2
apicid      : 0
initial apicid  : 0
fdiv_bug    : no
f00f_bug    : no
coma_bug    : no
fpu     : yes
fpu_exception   : yes
cpuid level : 13
wp      : yes
flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm pcid sse4_1 sse4_2 movbe popcnt tsc_deadline_timer xsave rdrand lahf_lm abm arat epb xsaveopt pln pts dtherm tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust erms invpcid
bogomips    : 5586.81
clflush size    : 64
cache_alignment : 64
address sizes   : 39 bits physical, 48 bits virtual
power management:

processor   : 1
vendor_id   : GenuineIntel
cpu family  : 6
model       : 60
model name  : Intel(R) Celeron(R) CPU G1840 @ 2.80GHz
stepping    : 3
microcode   : 0x1d
cpu MHz     : 800.000
cache size  : 2048 KB
physical id : 0
siblings    : 2
core id     : 1
cpu cores   : 2
apicid      : 2
initial apicid  : 2
fdiv_bug    : no
f00f_bug    : no
coma_bug    : no
fpu     : yes
fpu_exception   : yes
cpuid level : 13
wp      : yes
flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm pcid sse4_1 sse4_2 movbe popcnt tsc_deadline_timer xsave rdrand lahf_lm abm arat epb xsaveopt pln pts dtherm tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust erms invpcid
bogomips    : 5586.81
clflush size    : 64
cache_alignment : 64
address sizes   : 39 bits physical, 48 bits virtual
power management:

ubuntu@studio:~$ 
```

[zol](http://pc.zol.com.cn/520/5206041.html)


主意**笔记本转型**骗局。


### Reference

- zhihu: [买笔记本那些五花八门的配置怎么看？都代表什么意思？](http://www.zhihu.com/question/34923596/answer/60818825)
- [科普向，电脑配置的价格差异 ](http://bbs.nga.cn/read.php?tid=8581289)
- [CPU的选择，体验/性能/功耗的妥协](https://zhuanlan.zhihu.com/p/20127546)

