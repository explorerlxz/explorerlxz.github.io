---
layout: post
title:  "Interesting shell commands"
date:   2017-07-22
---

I downloaded [linux kernel 4.12.3 tarball](https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.12.3.tar.xz), and found there are 64052 file in the tarball, now I want to know exactly how many kind of files there, how many files of each kind.

I run the following command:


{% highlight sh %}
tar -tf linux-4.12.3.tar.xz > linux-4.12.3-dir.tree
cat linux-4.12.3-dir.tree | grep -E -o '*\.[A-Za-z0-9]{1,19}$' | sort -n |uniq -c | sort -nr
{% endhighlight %}



<div onclick="ishidden('X')">Then I got this result, Press here to see 206 more lines</div>
<div id="X" style="display:none;">
  24934 .c<br />
  19613 .h<br />
   3817 .txt<br />
   1441 .S<br />
   1330 .dts<br />
   1024 .dtsi<br />
    763 .rst<br />
    201 .gitignore<br />
    165 .sh<br />
    162 .json<br />
    109 .ihex<br />
     63 .py<br />
     59 .cocci<br />
     46 .boot<br />
     45 .svg<br />
     40 .tc<br />
     37 .pl<br />
     32 .config<br />
     30 .debug<br />
     25 .HEX<br />
     19 .lds<br />
     15 .tmpl<br />
     15 .conf<br />
     14 .ppm<br />
     14 .fuc<br />
     12 .fuc3<br />
     10 .exceptions<br />
     10 .awk<br />
      9 .y<br />
      8 .l<br />
      8 .8<br />
      8 .1<br />
      7 .scr<br />
      7 .sa<br />
      7 .in<br />
      7 .H16<br />
      7 .dot<br />
      6 .xsl<br />
      6 .cpp<br />
      6 .asn1<br />
      5 .uc<br />
      5 .po<br />
      5 .inc<br />
      5 .fuc5<br />
      5 .cpu<br />
      4 .tbl<br />
      4 .map<br />
      4 .ld<br />
      4 .include<br />
      4 .fail<br />
      4 .doc<br />
      3 .sed<br />
      3 .pm<br />
      3 .mk<br />
      3 .html<br />
      3 .gdbinit<br />
      3 .csv<br />
      3 .am<br />
      2 .um<br />
      2 .ubsan<br />
      2 .seq<br />
      2 .rules<br />
      2 .reg<br />
      2 .powerpc<br />
      2 .postlink<br />
      2 .platforms<br />
      2 .platform<br />
      2 .pbm<br />
      2 .openrisc<br />
      2 .nommu<br />
      2 .megaraid<br />
      2 .kasan<br />
      2 .inl<br />
      2 .inf<br />
      2 .ids<br />
      2 .gperf<br />
      2 .fax<br />
      2 .FAQ<br />
      2 .build<br />
      2 .asm<br />
      2 .arm<br />
      1 .ymfsb<br />
      1 .xs<br />
      1 .x86<br />
      1 .x25<br />
      1 .wimax<br />
      1 .WARNING<br />
      1 .vringh<br />
      1 .vim<br />
      1 .vdec2<br />
      1 .uni<br />
      1 .tex<br />
      1 .syncppp<br />
      1 .sym53c8xx<br />
      1 .SRC<br />
      1 .sphinx<br />
      1 .soc<br />
      1 .smp<br />
      1 .script<br />
      1 .sb1000<br />
      1 .rest<br />
      1 .README<br />
      1 .qlge<br />
      1 .qlcnic<br />
      1 .qla4xxx<br />
      1 .qla3xxx<br />
      1 .qla2xxx<br />
      1 .preempt<br />
      1 .PL<br />
      1 .perf<br />
      1 .pass<br />
      1 .OSS<br />
      1 .ore<br />
      1 .normal<br />
      1 .netlink<br />
      1 .net<br />
      1 .ncr53c8xx<br />
      1 .modules<br />
      1 .modsign<br />
      1 .modpost<br />
      1 .modinst<br />
      1 .modes<br />
      1 .modbuiltin<br />
      1 .mm<br />
      1 .mISDN<br />
      1 .mips<br />
      1 .md<br />
      1 .mak<br />
      1 .mailmap<br />
      1 .machine<br />
      1 .lpfc<br />
      1 .loopback<br />
      1 .locks<br />
      1 .Locking<br />
      1 .libfdt<br />
      1 .LIB<br />
      1 .lib<br />
      1 .kmemcheck<br />
      1 .kgdb<br />
      1 .ipw2200<br />
      1 .ipw2100<br />
      1 .ips<br />
      1 .iosched<br />
      1 .ignore<br />
      1 .i2400m<br />
      1 .hz<br />
      1 .hysdn<br />
      1 .hp300<br />
      1 .host<br />
      1 .HiSax<br />
      1 .help<br />
      1 .headersinst<br />
      1 .glade<br />
      1 .gitattributes<br />
      1 .gigaset<br />
      1 .gif<br />
      1 .generic<br />
      1 .gate<br />
      1 .fwinst<br />
      1 .fuc4<br />
      1 .fuc0s<br />
      1 .freezer<br />
      1 .FPE<br />
      1 .FlashPoint<br />
      1 .FIRST<br />
      1 .feature<br />
      1 .extrawarn<br />
      1 .example<br />
      1 .dtc<br />
      1 .dtbinst<br />
      1 .DOC<br />
      1 .diversion<br />
      1 .dino<br />
      1 .devices<br />
      1 .default<br />
      1 .def<br />
      1 .DAC960<br />
      1 .cycladesZ<br />
      1 .css<br />
      1 .cputype<br />
      1 .copyright<br />
      1 .concap<br />
      1 .common<br />
      1 .cocciconfig<br />
      1 .clean<br />
      1 .checkpatch<br />
      1 .char<br />
      1 .ChangeLog<br />
      1 .cfg<br />
      1 .cert<br />
      1 .cc<br />
      1 .CAPI<br />
      1 .cache<br />
      1 .bus<br />
      1 .buddha<br />
      1 .binfmt<br />
      1 .bc<br />
      1 .avmb1<br />
      1 .audio<br />
      1 .arcmsr<br />
      1 .arch<br />
      1 .aic7xxx<br />
      1 .aic79xx<br />
      1 .agh<br />
      1 .AddingFirmware<br />
      1 .ac<br />
</div>


### commands analysis

{% highlight sh %}
tar -tf linux-4.12.3.tar.xz > linux-4.12.3-dir.tree
cat linux-4.12.3-dir.tree | grep -E -o '*\.[A-Za-z0-9]{1,19}$' | sort -n |uniq -c | sort -nr
{% endhighlight %}

```shell
tar -tf linux-4.12.3.tar.xz //list the contents of an archive
cat linux-4.12.3-dir.tree   //concatenate files and print on the standard output
grep -E -o '*\.[A-Za-z0-9]{1,19}$'  //get the file type info
sort -n                             //sort lines of text files, compare according to string numerical value
uniq -c                             //report or omit repeated lines, prefix lines by the number of occurrences
sort -nr                            //sort lines of text files, compare according to string numerical value, reverse the result of comparisons
```
