---
layout: post
title: "樹莓派GPIO方面"
date: 2016-11-18
---

## 通過樹莓派的GPIO控制步進電機

#### Raspberry Pi 3B GPIO pin table
 
| BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
|---:|---:|---:|---:|---:|:---:|:---|:---|:---|:---|:---|
|     |     |    3.3v |      |   |  1   2  |   |      | 5v      |     |     |
|   2 |   8 |   SDA.1 |   IN | 1 |  3   4  |   |      | 5V      |     |     |
|   3 |   9 |   SCL.1 |   IN | 1 |  5   6  |   |      | 0v      |     |     |
|   4 |   7 | GPIO. 7 |   IN | 1 |  7   8  | 0 | IN   | TxD     | 15  | 14  |
|     |     |      0v |      |   |  9   10 | 1 | IN   | RxD     | 16  | 15  |
|  17 |   0 | GPIO. 0 |  OUT | 0 | 11   12 | 0 | OUT  | GPIO. 1 | 1   | 18  |
|  27 |   2 | GPIO. 2 |  OUT | 0 | 13   14 |   |      | 0v      |     |     |
|  22 |   3 | GPIO. 3 |  OUT | 1 | 15   16 | 0 | IN   | GPIO. 4 | 4   | 23  |
|     |     |    3.3v |      |   | 17   18 | 0 | IN   | GPIO. 5 | 5   | 24  |
|  10 |  12 |    MOSI |   IN | 0 | 19   20 |   |      | 0v      |     |     |
|   9 |  13 |    MISO |   IN | 0 | 21   22 | 0 | IN   | GPIO. 6 | 6   | 25  |
|  11 |  14 |    SCLK |   IN | 0 | 23   24 | 1 | IN   | CE0     | 10  | 8   |
|     |     |      0v |      |   | 25   26 | 1 | IN   | CE1     | 11  | 7   |
|   0 |  30 |   SDA.0 |   IN | 1 | 27   28 | 1 | IN   | SCL.0   | 31  | 1   |
|   5 |  21 | GPIO.21 |   IN | 1 | 29   30 |   |      | 0v      |     |     |
|   6 |  22 | GPIO.22 |   IN | 1 | 31   32 | 0 | IN   | GPIO.26 | 26  | 12  |
|  13 |  23 | GPIO.23 |   IN | 0 | 33   34 |   |      | 0v      |     |     |
|  19 |  24 | GPIO.24 |  OUT | 1 | 35   36 | 0 | OUT  | GPIO.27 | 27  | 16  |
|  26 |  25 | GPIO.25 |   IN | 0 | 37   38 | 0 | OUT  | GPIO.28 | 28  | 20  |
|     |     |      0v |      |   | 39   40 | 0 | OUT  | GPIO.29 | 29  | 21  |
| BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |

控制步進電機需要步進電機驅動板，驅動板有4個輸入口:IN1~IN4，通過這四個口來連接樹莓派的4個GPIO口，驅動板的電源正負極分別接在樹莓派的Pin 2口和Pin 6口。如下表所示：

樹莓派引腳|Pin 2(+5V)|Pin 6(GND)|11(GPIO 0)|Pin 12(GPIO 1)|Pin 13(GPIO 2)|Pin 15(GPIO 3) 
:-:|:-:|:-:|:-:|:-:|:-:|:-:
驅動板|正極|負極|IN1|IN2|IN3|IN4


## C語言版本

C語言版本要用到wiringPi這個庫，具體命令如下：

```
git clone git://git.drogon.net/wiringPi
cd wiringPi
./build
```

 C程序的代碼如下

```C
/* moto.c
* A program to control a stepper motor through the GPIO on Raspberry Pi. 
* 
* Author: Darran Zhang (http://www.codelast.com) 
*/
#include <wiringPi.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#define CLOCKWISE 1
#define COUNTER_CLOCKWISE 2
void delayMS(int x);
void rotate(int* pins, int direction);
int main(int argc,char* argv[]) {
    if (argc < 4) {
        printf("Usage example: ./motor 0 1 2 3 \n");
        return 1;
    }
    /* number of the pins which connected to the stepper motor driver board */
    int pinA = atoi(argv[1]);
    int pinB = atoi(argv[2]);
    int pinC = atoi(argv[3]);
    int pinD = atoi(argv[4]);
    int pins[4] = {pinA, pinB, pinC, pinD};
    if (-1 == wiringPiSetup()) {
        printf("Setup wiringPi failed!");
        return 1;
    }
    /* set mode to output */
    pinMode(pinA, OUTPUT);
    pinMode(pinB, OUTPUT);
    pinMode(pinC, OUTPUT);
    pinMode(pinD, OUTPUT);
    delayMS(50);    // wait for a stable status 
    for (int i = 0; i < 500; i++) {
        rotate(pins, CLOCKWISE);
    }
    return 0;
}

/* Suspend execution for x milliseconds intervals.
* @param ms Milliseconds to sleep.
*/
void delayMS(int x) {
    usleep(x * 1000);
}

/* Rotate the motor.
* @param pins     A pointer which points to the pins number array.
*  @param direction CLOCKWISE for clockwise rotation, COUNTER_CLOCKWISE for counter clockwise rotation.
*/
void rotate(int* pins, int direction) {
    for (int i = 0; i < 4; i++) {
        if (CLOCKWISE == direction) {
            for (int j = 0; j < 4; j++) {
                if (j == i) {
                    digitalWrite(pins[3 - j], 1); // output a high level 
                } else {
                    digitalWrite(pins[3 - j], 0); // output a low level 
                }
            }
        } else if (COUNTER_CLOCKWISE == direction) {
            for (int j = 0; j < 4; j++) {
                if (j == i) {
                    digitalWrite(pins[j], 1); // output a high level 
                } else {
                    digitalWrite(pins[j], 0); // output a low level 
                }
            }
        }
        delayMS(4);
    }
}
```

編譯程序：

```
g++ motor.c -o motor -lwiringPi
```

運行程序:

逆時針旋轉360度

```
sudo ./motor 0 1 2 3
```
順時針旋轉360度

```
sudo ./motor 3 2 1 0
```

## Python版本

Python版本要用到rpi.gpio，raspbian系統默認已經安裝了

```
$ sudo apt-get update
$ sudo apt-get install python-rpi.gpio python3-rpi.gpio
```

Python代碼如下：

```Python
#coding: utf8
import RPi.GPIO as GPIO
import time
import sys
from array import *

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)

steps    = int(sys.argv[1]);
clockwise = int(sys.argv[2]);

arr = [0,1,2,3];
if clockwise!=1:
    arr = [3,2,1,0];

ports = [11,12,13,15] # GPIO 0（Pin 11） GPIO 1（Pin 12） GPIO 2（Pin 13） GPIO 3（Pin 15）

for p in ports:
    GPIO.setup(p,GPIO.OUT)

for x in range(0,steps):
    for j in arr:
        time.sleep(0.01)
        for i in range(0,4):
            if i == j:
                GPIO.output(ports[i],True)
            else:
                GPIO.output(ports[i],False)
```

逆時針旋轉約80度

```
sudo python motor.py 90 0
```

順時針旋轉約80度

```
sudo python motor.py 90 1
```

## Reference
 - Wiring Pi:[下載安裝wiringPi](http://wiringpi.com/download-and-install/)
 - [升級RPi.GPIO](https://sourceforge.net/p/raspberry-gpio-python/wiki/install/)
 - CSDN:[树莓派通过GPIO控制步进电机(python) ](http://blog.csdn.net/u010027419/article/details/41518321)
 - [通过Raspberry Pi(树莓派)的GPIO接口控制步进电机](http://www.codelast.com/%E5%8E%9F%E5%88%9B%E9%80%9A%E8%BF%87raspberry-pi%E6%A0%91%E8%8E%93%E6%B4%BE%E7%9A%84gpio%E6%8E%A5%E5%8F%A3%E6%8E%A7%E5%88%B6%E6%AD%A5%E8%BF%9B%E7%94%B5%E6%9C%BAcontrol-stepper-motor-through-the-g/)
 - 極客范：[Adafruit的树莓派教程第十课：步进电机](http://www.geekfan.net/9926/)
