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


Then I got this result:

<div onclick="ishidden('X')">有东西藏起来了!</div>
<div id="X" style="display:none;">
```
  24934 .c
  19613 .h
   3817 .txt
   1441 .S
   1330 .dts
   1024 .dtsi
    763 .rst
    201 .gitignore
    165 .sh
    162 .json
    109 .ihex
     63 .py
     59 .cocci
     46 .boot
     45 .svg
     40 .tc
     37 .pl
     32 .config
     30 .debug
     25 .HEX
     19 .lds
     15 .tmpl
     15 .conf
     14 .ppm
     14 .fuc
     12 .fuc3
     10 .exceptions
     10 .awk
      9 .y
      8 .l
      8 .8
      8 .1
      7 .scr
      7 .sa
      7 .in
      7 .H16
      7 .dot
      6 .xsl
      6 .cpp
      6 .asn1
      5 .uc
      5 .po
      5 .inc
      5 .fuc5
      5 .cpu
      4 .tbl
      4 .map
      4 .ld
      4 .include
      4 .fail
      4 .doc
      3 .sed
      3 .pm
      3 .mk
      3 .html
      3 .gdbinit
      3 .csv
      3 .am
      2 .um
      2 .ubsan
      2 .seq
      2 .rules
      2 .reg
      2 .powerpc
      2 .postlink
      2 .platforms
      2 .platform
      2 .pbm
      2 .openrisc
      2 .nommu
      2 .megaraid
      2 .kasan
      2 .inl
      2 .inf
      2 .ids
      2 .gperf
      2 .fax
      2 .FAQ
      2 .build
      2 .asm
      2 .arm
      1 .ymfsb
      1 .xs
      1 .x86
      1 .x25
      1 .wimax
      1 .WARNING
      1 .vringh
      1 .vim
      1 .vdec2
      1 .uni
      1 .tex
      1 .syncppp
      1 .sym53c8xx
      1 .SRC
      1 .sphinx
      1 .soc
      1 .smp
      1 .script
      1 .sb1000
      1 .rest
      1 .README
      1 .qlge
      1 .qlcnic
      1 .qla4xxx
      1 .qla3xxx
      1 .qla2xxx
      1 .preempt
      1 .PL
      1 .perf
      1 .pass
      1 .OSS
      1 .ore
      1 .normal
      1 .netlink
      1 .net
      1 .ncr53c8xx
      1 .modules
      1 .modsign
      1 .modpost
      1 .modinst
      1 .modes
      1 .modbuiltin
      1 .mm
      1 .mISDN
      1 .mips
      1 .md
      1 .mak
      1 .mailmap
      1 .machine
      1 .lpfc
      1 .loopback
      1 .locks
      1 .Locking
      1 .libfdt
      1 .LIB
      1 .lib
      1 .kmemcheck
      1 .kgdb
      1 .ipw2200
      1 .ipw2100
      1 .ips
      1 .iosched
      1 .ignore
      1 .i2400m
      1 .hz
      1 .hysdn
      1 .hp300
      1 .host
      1 .HiSax
      1 .help
      1 .headersinst
      1 .glade
      1 .gitattributes
      1 .gigaset
      1 .gif
      1 .generic
      1 .gate
      1 .fwinst
      1 .fuc4
      1 .fuc0s
      1 .freezer
      1 .FPE
      1 .FlashPoint
      1 .FIRST
      1 .feature
      1 .extrawarn
      1 .example
      1 .dtc
      1 .dtbinst
      1 .DOC
      1 .diversion
      1 .dino
      1 .devices
      1 .default
      1 .def
      1 .DAC960
      1 .cycladesZ
      1 .css
      1 .cputype
      1 .copyright
      1 .concap
      1 .common
      1 .cocciconfig
      1 .clean
      1 .checkpatch
      1 .char
      1 .ChangeLog
      1 .cfg
      1 .cert
      1 .cc
      1 .CAPI
      1 .cache
      1 .bus
      1 .buddha
      1 .binfmt
      1 .bc
      1 .avmb1
      1 .audio
      1 .arcmsr
      1 .arch
      1 .aic7xxx
      1 .aic79xx
      1 .agh
      1 .AddingFirmware
      1 .ac
```
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
