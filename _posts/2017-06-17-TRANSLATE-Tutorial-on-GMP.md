---
layout: post
title:  "[译]GMP教程"
date:   2017-06-16
---

原文链接：[Tutorial on GMP](https://www.cs.colorado.edu/~srirams/courses/csci2824-spr14/gmpTutorial.html)


# Tutorial on GMP

---
---


GMP代表着Gnu MultiPrecision Library。它是一个非常流行的可以让我们操作任意精度整数、有理数和浮点数的库：MPFR库的出现对于任意精度浮点数非常有用。

本教程主要关注该库的C语言方面。对C语言的扩展非常容易使用。但从该库C语言的使用开始让我们更好的控制我们可以做什么，同时也让我们更好的理解该库的工作方式。


## Getting Started

---

我假设你有一个装有GMP库的linux系统。GMP可以从[gmplib.org](gmplib.org)下载。

最新的用户指南可以在[这里](http://gmplib.org/manual)找到。

## 什么是任意精度？为啥我们需要它？

---

电脑每次可以处理一个32或64位数字。如果你用C语言写一个程序，声明**int x**, 这个**int**就是一个32位数据或者64位数据。这取决于你的硬件和编译器。

用C语言写一个像下面一样的小程序你就会发现所有真相：


#### Native int

```C
#include <stdio.h>
#include <assert.h>
#include <limits.h>

int main(){
   int k,x,i;

   /* 1. Print the machine native <span class="builtin">int</span> size and the max/min integer */
   printf ("Size of integers in this computer = %d bits \n", sizeof(int) * 8);
   printf ("The largest int representable is %d \n", INT_MAX);
   printf ("The smallest int representable is %d \n", INT_MIN);

   /* 2. Let us add 1 to INT_MAX */
   k = INT_MAX;

   printf (" %d + 1 = %d \n", k,k+1);

   /* 3. Input a number x and print its binary representation out */

   printf ("Enter x:");
   scanf("%d",&x);
   printf ("The binary reprentation LSB --> MSB is: ");
   for (i=0; i < 8 * sizeof(int); ++i){

       if (x %2 == 0)
          printf ("0");
       else
          printf("1");

       x = x >> 1; // Shift right by 1

   }

   printf ("\n");

}

```

**sizeof(int)**返回一个整数所占的字节数。**INT_MAX**是**limits.h**里定义的一个代表最大整数的宏。**INT_MIN**也是类似的。

我在MAC上编译后得到下面的结果。

### Output on a MAC Laptop

```
Size of integers in this computer = 32 bits
The largest int representable is 2147483647
The smallest int representable is -2147483648
 2147483647 + 1 = -2147483648
Enter x:3091
The binary reprentation LSB --> MSB is: 11001000001100000000000000000000
```

请注意，计算机可以表示**-2147483648**到**2137483647**之间的整数。任何尝试给最大数加1的结果都是返回到最小数。

对于很多需要表示更大数的程序来说简直就是惩罚。例如，银行必须考虑到如果某个人有20多亿美元，如果他再多存一美元，账户上就成负数了。

C语言也有一个**long**类型支持64位运算。事实上，当前制造的intel处理器有64位字单元。换到64位大概可以给我们返回一个**LONG_MAX**为10进制20位的数。但这还是个限制。

下面是一个更有趣的精度极限演示。

### 阶乘运算

```C
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>

int fact(int n){
  /*
     Function: fact (n)
     Returns: n!
  */

  int i;
  int p = 1;

  for (i=1; i <= n ; ++i){
    p = p * i;
  }
  return p;

}

int main(int argc, char * argv[]){
  int n;

  if (argc <= 1){
    /* Check if user has a command line argument */
    printf ("Usage: %s <number>", argv[0]);
    return 2;
  }

  n = atoi(argv[1]); /* Extract the command line argument */

  assert( n >= 0);

  /* Print the factorial */
  printf ("%d ! = %d \n", n, fact(n));

  return 1;
}
```

我们把上面的程序编译出来玩一下。

### 玩一玩阶乘

```
bash-3.2$ gcc -o fact fact.c
bash-3.2$ ./fact
Usage: ./fact <number>
bash-3.2$ ./fact 15
15 ! = 2004310016
bash-3.2$ ./fact 16
16 ! = 2004189184
bash-3.2$ ./fact 17
17 ! = -288522240
bash-3.2$ ./fact 18
18 ! = -898433024
bash-3.2$ ./fact 19
19 ! = 109641728
bash-3.2$ ./fact 20
20 ! = -2102132736
bash-3.2$ ./fact 21
21 ! = -1195114496
bash-3.2$ ./fact 22
22 ! = -522715136
bash-3.2$ ./fact 23
23 ! = 862453760
bash-3.2$ ./fact 24
24 ! = -775946240
bash-3.2$ ./fact 25
25 ! = 2076180480
bash-3.2$ ./fact 26
26 ! = -1853882368
bash-3.2$ ./fact 27
27 ! = 1484783616
bash-3.2$ ./fact 28
28 ! = -1375731712
bash-3.2$
```

17!怎么会是负数？:-)

## 无限精度运算

---

GMP允许我们使用可以动态变化达到要求精度的整数。总而言之，不管128位，256位还是1024位都可以支持。我们没有必要声明需要多少位的整数。该库会根据需要动态分配内存达到需要的精度。

## 在电脑上安装GMP库

这是最难的一部分：(a)在电脑上安装GMP相关的东西(b)集成到你用的C编译器或者C程序中。

我个人有linux、windows下CYGWIN和MAC OSX下macports的使用经验。

在linux主机上，最简单的方法是使用apt-get、rpm或yum之类的包管理工具安装.Redhat系统使用RPM然而在ubuntu系统下却用apt-get。如果你使用debian系统，那么可以使用dpkg安装。

如果你的系统被其它管理员管理(eg.,CSEL实验室管理员)，那么要求管理员安装gmp。GMP将被所有的用户使用。你可以先检查一下

### 看看我的MAC OSX是不是安装了GMP

```
bash-3.2$ locate libgmp.a
  /sw/lib/libgmp.a
```

办公室的电脑使用linux系统

### 办公室电脑

```
srirams@turing:~$ locate libgmp.a
/usr/lib/libgmp.a
```

## GMP整数基础

下面的程序读取一个数字，加1后输出结果。

### 使用GMP整数API的简单程序

```
#include <gmp.h>
#include <stdio.h>
#include <assert.h>

int main(){

  char inputStr[1024];
  /*
     mpz_t is the type defined for GMP integers.
     It is a pointer to the internals of the GMP integer data structure
   */
  mpz_t n;
  int flag;

  printf ("Enter your number: ");
  scanf("%1023s" , inputStr); /* NOTE: never every write a call scanf ("%s", inputStr);
                                  You are leaving a security hole in your code. */

  /* 1. Initialize the number */
  mpz_init(n);
  mpz_set_ui(n,0);

  /* 2. Parse the input string as a base 10 number */
  flag = mpz_set_str(n,inputStr, 10);
  assert (flag == 0); /* If flag is not 0 then the operation failed */

  /* Print n */
  printf ("n = ");
  mpz_out_str(stdout,10,n);
  printf ("\n");

  /* 3. Add one to the number */

  mpz_add_ui(n,n,1); /* n = n + 1 */

  /* 4. Print the result */

  printf (" n +1 = ");
  mpz_out_str(stdout,10,n);
  printf ("\n");


  /* 5. Square n+1 */

  mpz_mul(n,n,n); /* n = n * n */


  printf (" (n +1)^2 = ");
  mpz_out_str(stdout,10,n);
  printf ("\n");


  /* 6. Clean up the mpz_t handles or else we will leak memory */
  mpz_clear(n);

}

```

编译代码（假定GMP全局安装在“标准”路径）

```
$ gcc -o mpz_simple1 mpz_simple1.c -lgmp
```

如果GMP没有安装在标准路径，那么你必须找到gmp.h和libgmp.a的位置再编译。在我笔记本上运行如下。

```
$ locate "/gmp.h"
/sw/include/gmp.h
$ locate "libgmp.a"
/sw/lib/libgmp.a
$ gcc -o mpz_simple1 -I /sw/include -L/sw/lib mpz_simple1.c -lgmp
```

如果没有差错，你会得到一个可执行程序。在这里，也就是**mpz_simple1**。

```
$ ./mpz_simple1
Enter your number: 6
n = 6
 n +1 = 7
  (n +1)^2 = 49
```

如果你想输入一个超级变态的n，那也没有任何问题 :-)

```
$ ./mpz_simple1
Enter your number: 1981098309851092850192851029581029581209581092581209581209581029581209581209856120856120958120958120958120958
n = 1981098309851092850192851029581029581209581092581209581209581029581209581209856120856120958120958120958120958
 n +1 = 1981098309851092850192851029581029581209581092581209581209581029581209581209856120856120958120958120958120959
  (n +1)^2 = 39247505132948566943624540368331641284... a really long number...
```

想要看懂上面的代码，你需要参考GMP手册的整数函数：[GMP Integer Functions](http://gmplib.org/manual/Integer-Functions.html)。

下面是程序的样板。

```C
#include <gmp.h>

/* la di da di da ... */

  mpz_t n;
  mpz_init(n);

      /* blah blah */

  mpz_set_ui(n,0);

      /* blah blah */

  mpz_clear(n);
```

**mpz_t** 是一个指向GMP中用于存储无限制精度整数的复杂数据结构的指针。我们不用管mpz_t的值是什么。但要记住它是一个指针。我们用它来处理整数。验证n的数值并没有什么用处。

**mpz_init(n)** 调用初始化函数，并且合理的初始化n。

**mpz_set_ui(n,0)** 把数值n赋值为0。

**mpz_clear(n)** 清除存储n所占据的内存单元.

## GMP阶乘代码

### 无限精度的阶乘

```C
#include "gmp.h"
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>

void fact(int n){
  int i;
  mpz_t p;

  mpz_init_set_ui(p,1); /* p = 1 */
  for (i=1; i <= n ; ++i){
    mpz_mul_ui(p,p,i); /* p = p * i */
  }
  printf ("%d!  =  ", n);
  mpz_out_str(stdout,10,p);
  mpz_clear(p);

}

int main(int argc, char * argv[]){
  int n;


  if (argc <= 1){
    printf ("Usage: %s <number> \n", argv[0]);
    return 2;
  }

  n = atoi(argv[1]);
  assert( n >= 0);
  fact(n);


  return 1;
}

```

上面的代码解决了使用原始int阶乘函数溢出的问题。

```
$ gcc -I /sw/include/ -L/sw/lib -o mpz_fact mpz_fact.c -lgmp
$ ./mpz_fact
Usage: ./mpz_fact <number>
$ ./mpz_fact 1093
1093!  =  279512617852590467904571737438372389.... and a buzillion more digits ;-)

$

$ ./mpz_fact 109
109!  =  1443859583202493582204882102462... goes on and on and on.. :-)

$ ./mpz_fact 10
10!  =  3628800
$ ./mpz_fact 15
15!  =  1307674368000
$ ./mpz_fact 16
16!  =  20922789888000
$ ./mpz_fact 17
17!  =  355687428096000
$ ./mpz_fact 19
19!  =  121645100408832000
$ ./mpz_fact 2091
2091!  = 64536472071648725652592612640419881740...and a guzzillion more digits :-)
```

### 有用的GMP整数函数

GMP提供了大量有用的整数函数：[Integer Arithmetic](http://gmplib.org/manual/Integer-Arithmetic.html#Integer-Arithmetic)
在这里可以发现大量有用的数论函数：[这儿](http://gmplib.org/manual/Number-Theoretic-Functions.html#Number-Theoretic-Functions)
