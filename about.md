---
layout: page
title: About
permalink: /about/
---

|书名|作者|关键字|评分|
|:-:|:-|:-|:-|
|《人生的智慧》|叔本华|人生、独处、痛苦、无聊|8|
|《二手时间》|阿列克谢耶维奇|苏联、苦难、人性|9|
|《UNIX痛恨者手册》|Simson Garfinkel|UNIX、缺点|8|
|《病隙碎笔》|史铁生|宗教、爱情、信仰、思考|9|
|《莫言文集》|莫言|乡村、苦难、思考|8|
|《慕容雪村经典文集》|慕容雪村|他到传销组织卧底写书，在下也是服了|7|
|《李敖大全集》|李敖|自传、评论、小说、历史|8|

王垠有一个观点我是比较认同的，不要轻易的相信什么权威。

我以前相信Visual Basic是非常垃圾的一种语言，相信\<Code Complete\>是非常棒的一本书。但等我打开这本书时，却发现其中有很多Visual Basic的例子。

我以前相信C语言不应该使用`goto`，函数应该尽可能短小，嵌套不要太多……可是当我查看OpenSSL,nginx,linux源码时，发现到处都是`goto`，到处都是`if else`，一个函数也需要数百行，这在某些权威口中是很low的程序员的表现，但是世界上多少大公司在使用OpenSSL呢？


#### linux4.11.7内核源码goto出现情况

Linux内核以及nginx源码goto出现的频率约为每百行一次，OpenSSL每百行5次，均为不完全统计。

```
[explorer@study linux-4.11.7]$ cd kernel/
[explorer@study kernel]$ ls
acct.c              extable.c         Makefile           stacktrace.c
async.c             fork.c            membarrier.c       stop_machine.c
audit.c             freezer.c         memremap.c         sys.c
auditfilter.c       futex.c           module.c           sysctl_binary.c
audit_fsnotify.c    futex_compat.c    module-internal.h  sysctl.c
audit.h             gcov              module_signing.c   sys_ni.c
auditsc.c           groups.c          notifier.c         taskstats.c
audit_tree.c        hung_task.c       nsproxy.c          task_work.c
audit_watch.c       irq               padata.c           test_kprobes.c
backtracetest.c     irq_work.c        panic.c            time
bounds.c            jump_label.c      params.c           torture.c
bpf                 kallsyms.c        pid.c              trace
capability.c        kcmp.c            pid_namespace.c    tracepoint.c
cgroup              Kconfig.freezer   power              tsacct.c
compat.c            Kconfig.hz        printk             ucount.c
configs             Kconfig.locks     profile.c          uid16.c
configs.c           Kconfig.preempt   ptrace.c           up.c
context_tracking.c  kcov.c            range.c            user.c
cpu.c               kexec.c           rcu                user_namespace.c
cpu_pm.c            kexec_core.c      reboot.c           user-return-notifier.c
crash_dump.c        kexec_file.c      relay.c            utsname.c
cred.c              kexec_internal.h  resource.c         utsname_sysctl.c
debug               kmod.c            sched              watchdog.c
delayacct.c         kprobes.c         seccomp.c          watchdog_hld.c
dma.c               ksysfs.c          signal.c           workqueue.c
elfcore.c           kthread.c         smpboot.c          workqueue_internal.h
events              latencytop.c      smpboot.h
exec_domain.c       livepatch         smp.c
exit.c              locking           softirq.c
[explorer@study kernel]$ vi cpu.c
[explorer@study kernel]$ cat *.c | wc -l
74167
[explorer@study kernel]$ cat *.c | grep goto | wc -l
733
[explorer@study kernel]$

```





### 健身书

|书名|作者|评分|
|:-:|:-|:-:|
|《奈特人体解剖学》|奈特|8|

