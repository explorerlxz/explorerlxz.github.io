---
layout: post
title:  "GMP库学习笔记，可用于RAS加密算法"
date:   2017-06-16
---

前一阵子一直在考虑如何使用RSA算法，一直找不到门路，虽然从网上下载了一些大数阶乘的程序，但毕竟不能解决所有问题，我的C语言学的并不好，不至于自己把整个库开发出来，了解有个openssl，又不知道怎么用到项目中，只好作罢了。

今天研究Shadowsocks源码时看到一篇博客[Cryptography解题报告：Factor the RSA modulus](https://lixingcong.github.io/2016/04/03/Cryptography-I-week-6/)，于是下载gmp库，编译安装，跟安装其它程序没啥区别。

```
./configure
make
make check
make install
```

安装完成后下载了例程

{% highlight cpp linenos %}
#include <iostream>
#include <gmp.h>
#include <cstring>


using namespace std;

char str_N[311]="179769313486231590772930519078902473361797697894230657273430081157732675805505620686985379449212982959585501387537164015710139858647833778606925583497541085196591615128057575940752635007475935288710823649949940771895617054361149474865046711015101563940680527540071584560878577663743040086340742855278549092581"; // 309digits

int main() {
    mp_exp_t mp_exp_t_exp;
    size_t n_digits=1200;           // æŒ‡å®šè¾“å‡ºstræ—¶çš„æœ‰æ•ˆæ•°å­—,å•ä½ï¼šå­—èŠ‚
    mp_bitcnt_t prec=1200;
    char res_p[500],res_q[500];
    mpf_set_default_prec(prec);
    
    mpf_t mpf_t_a,mpf_t_b;      // æµ®ç‚¹æ•°
    mpz_t mpz_t_a,mpz_t_b,mpz_t_c,mpz_t_d; // æ•´æ•°
    mpz_t mpz_t_N,mpz_t_A;                 // æ•´æ•°ï¼šå¸¸é‡Aå’ŒN
    mpf_init(mpf_t_a);
    mpf_init(mpf_t_b);
    mpz_init(mpz_t_a);
    mpz_init(mpz_t_A);
    mpz_init(mpz_t_b);
    mpz_init(mpz_t_c);
    mpz_init(mpz_t_d);
    mpz_init_set_str(mpz_t_N,str_N,10);
    mpf_init_set_str(mpf_t_b,str_N,10);
    
    mpf_sqrt(mpf_t_a,mpf_t_b);        // sqrt(N)å­˜å…¥mpf_a
    mpf_ceil(mpf_t_b,mpf_t_a);        // ceilåŽå¾—åˆ°æµ®ç‚¹æ•°å­˜å…¥mpf_b
    mpz_set_f(mpz_t_A,mpf_t_b);       // å°†ceilåŽæµ®ç‚¹æ•°è½¬æˆæ•´æ•°å­˜å…¥mpz_A
    mpz_pow_ui(mpz_t_d,mpz_t_A,2);    // A^2å­˜å…¥mpz_d
    mpz_sub(mpz_t_c,mpz_t_d,mpz_t_N); // A^2-Nå­˜å…¥mpz_c
    mpz_sqrt(mpz_t_b,mpz_t_c);        // x=sqrt(A^2-N)å­˜å…¥mpz_b
    mpz_sub(mpz_t_a,mpz_t_A,mpz_t_b); // p=A-xå­˜å…¥mpz_a
    mpz_add(mpz_t_c,mpz_t_A,mpz_t_b); // q=A+xå­˜å…¥mpz_c
    
    // mpz_mul(mpz_t_d,mpz_t_c,mpz_t_a); // p*qå­˜å…¥mpz_d
    mpz_get_str(res_p,10,mpz_t_a);
    cout<<"p = "<<res_p<<endl;
    mpz_get_str(res_q,10,mpz_t_c);
    cout<<"q = "<<res_q<<endl;
    if(mpz_cmp(mpz_t_a,mpz_t_c)>0){
        cout<<"q is smaller\n";
    }else
        cout<<"p is smaller\n";

    return 0;
}
{% endhighlight %}

编译程序时C++要用到libgmp和libgmpxx，如下：

```
g++ mycxxprog.cc -lgmpxx -lgmp -o a.out
```

刚开始完全搞不懂程序说的什么，一方面因为我对RSA破解原理了解甚少，一方面这种程序可读性太TM差了。我通过gmplib.org下载了文档，逐个函数搜索查看，终于理解个差不多，我还发现程序可以支持2~62进制数，简直太神奇了，有空一定好好研究gmp库的源码。

上面都是10进制，下面一个是2进制，一个62进制

{% highlight bash linenos %}
[explorer@study gmp]$ ./test1.out
p = 13407807929942597099574024998205846127479365820592393377723561443721764030073662768891111614362326998675040546094339320838419523375986027530441562135724301
q = 13407807929942597099574024998205846127479365820592393377723561443721764030073778560980348930557750569660049234002192590823085163940025485114449475265364281
p is smaller
[explorer@study gmp]$ ./sample.o
p = 100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100001101
q = xR9fAlrdKvCIINsqEkJZSfvkAt8lzmSSSSwEFE05v08Bb1pMyu61n2UITyWDeOOU4PPuyM9t73OAFXx6rEnE1B
p is smaller
[explorer@study gmp]$
{% endhighlight %}

