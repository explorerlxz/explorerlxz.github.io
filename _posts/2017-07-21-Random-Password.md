---
layout: post
title:  "生成隨機密碼"
date:   2017-07-21
---

## Shell Version

```
cat /dev/urandom|tr -dc "a-zA-Z0-9-_\$\?\*\%\$\@\(\)\_\+"|fold -w 20 |head -n 10
```

>3-fTP4rUr3b_dK?5QD1j
xGX?-GREFXtUA-1q$+$0
_ai2fve@HpVTlVbltD92
OSfH9Gc)5S6UbRNCPgVC
kKdsulm@@llPvH0rNkqq
v5ZHaRARcTsf(FvHuHnr
9L$2MSO-)RfIMB-tWdS6
ODoiAlU9w(2x5(W*BKxL
sqZmB(x)H_6*$0NnWGaV
_cPT$d0$2%w%l58Cyq*d

## C Version


```
#include <stdio.h>
#include <stdlib.h>
#include <time.h>  
//this is a program to generate a random password

int main()
{
    int counter = 0;
    srand(time(NULL));
    char randChar;

    int  passwordLength = 20;

//    printf("Type in a password Length \n");
//    scanf("%d", &passwordLength);

    int i;
    for(i=0;i<10;i++)
    {
        counter = 0;
        while(counter < passwordLength)
        {
            //seed random based on time
            randChar = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"[random () % 62];    
            printf("%c", randChar);
            counter++;
        }   
        printf("\n");
    }
    printf("\n");
    return 0;
}
```

>x4t3X6Hki6DJITCdyYr8
2HL2CF4WOCXC7F5U9A3h
74rDNtgCIM88TH0WKuqZ
wCbrFWAEV1mQvRT8Ax8I
74GRB4lMoPjZPAGSUEXp
575qOMnO8jV5eZKdUtn6
9MVMKcfdg0IavCG9MrLK
ReEt2OMK6yG3AcFJ2imW
YswJu1G5iQDzsHikUtuO
h1HfRKoHtQdF7QNpHTkp



## Python Version

```
import os, random, string

length = 20
chars = string.ascii_letters + string.digits + '!@#$%^&*()'
random.seed = (os.urandom(1024))

for num in range(0,9):
    print ''.join(random.choice(chars) for i in range(length)
```

>hTvHSrqb6yZr^ie7RFqQ
2tTmZy3lk^VE!oK@X8LK
9Wkhyv7ABf1&9IeWS6&9
@eR9O27HGhUburR%sU5t
jYxL^q2(RABnpnb7RGrR
QXz6guhRVts%LHIxa7&v
Ag7tx^wn3OBLy4OdSFa8
x4^E$1&aTbjXcbh((QMV
NsGYaCh3hJir^mnnOt(a


