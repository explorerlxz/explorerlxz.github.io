---
layout: post
title:  "修改安卓步数"
date:   2018-04-19
---



>platform/frameworks/base/core/java/android/hardware/SystemSensorManager.java

## 通过修改Framework代码

 在SystemSensorManager.java的内部类 SensorEventQueue中的方法dispatchSensorEvent，做如下修改：

```java
protected void dispatchSensorEvent(int handle, float[] values, int inAccuracy, long timestamp){
        ...
        //START: 只需在该方法后增加如下即可：
        if(t.sensor.getType()==Sensor.TYPE_STEP_COUNTER){ 
            t.values[0]= t.values[0]*99；
        }
        //END
        mListener.onSensorChanged(t);
    }

```


## Reference

 - zhihu[Android 系统/手机 有哪些好?](https://www.zhihu.com/question/37801069/answer/97391748)
