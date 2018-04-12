---
layout: post
title:  "LineageOS中相机(Snap)应用异常退出问题"
date:   2018-04-12
---


从去年开始，就会时不时的出现无法拍照的问题。打开相机，点拍照后提示“媒体服务器意外终止，正在关闭相机。”，去年还遇到过手机闹钟没声音，今年又遇到过无法播放音乐等问题。


相机程序的代码位于`packages/apps/Snap/`目录下，在资源文件中查找相关的字符串，在`res/values-zh-rCN/qcomstrings.xml`中找到了这一行：

```
  <string name="camera_server_died">媒体服务器意外终止，正在关闭相机。</string>
```


在整个项目中搜索`camera_server_died`，在`src/com/android/camera/CameraErrorCallback.java`文件:


```java
/*
 * Copyright (C) 2010 The Android Open Source Project
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.android.camera;

import android.util.Log;
import android.widget.Toast;
import com.android.camera.ui.RotateTextToast;
import org.codeaurora.snapcam.R;

public class CameraErrorCallback
        implements android.hardware.Camera.ErrorCallback {
    private static final String TAG = "CameraErrorCallback";
    public CameraActivity mActivity = null;
    //custom error code for thermal shutdown. This should be in sync
    //with HAL.
    private static final int THERMAL_SHUTDOWN = 50;

    public void setActivity(CameraActivity activity) {
        mActivity = activity;
    }

    @Override
    public void onError(int error, android.hardware.Camera camera) {
        Log.e(TAG, "Got camera error callback. error=" + error);
        // We are not sure about the current state of the app (in preview or
        // snapshot or recording). Closing the app is better than creating a
        // new Camera object.
        if (mActivity != null) {
            final int resId;
            switch (error) {
                case android.hardware.Camera.CAMERA_ERROR_SERVER_DIED:
                    resId = R.string.camera_server_died;
                    break;
                case THERMAL_SHUTDOWN:
                    resId = R.string.camera_thermal_shutdown;
                    break;
                case android.hardware.Camera.CAMERA_ERROR_UNKNOWN:
                default:
                    resId = R.string.camera_unknown_error;
                    break;
            }
            mActivity.runOnUiThread(new Runnable() {
                public void run() {
                     RotateTextToast.makeText(mActivity, resId, Toast.LENGTH_LONG).show();
                     mActivity.finish();
                }
            });
        } else {
            throw new RuntimeException("Unknown error");
        }
    }
}
```


在`frameworks/base/core/java/android/hardware/Camera.java`中找到了`CAMERA_ERROR_SERVER_DIED`的定义：


```java

    /**
     * Media server died. In this case, the application must release the
     * Camera object and instantiate a new one.
     * @see Camera.ErrorCallback
     */
    public static final int CAMERA_ERROR_SERVER_DIED = 100;

    /**
     * Callback interface for camera error notification.
     *
     * @see #setErrorCallback(ErrorCallback)
     *
     * @deprecated We recommend using the new {@link android.hardware.camera2} API for new
     *             applications.
     */
    @Deprecated
    public interface ErrorCallback
    {
        /**
         * Callback for camera errors.
         * @param error   error code:
         * <ul>
         * <li>{@link #CAMERA_ERROR_UNKNOWN}
         * <li>{@link #CAMERA_ERROR_SERVER_DIED}
         * </ul>
         * @param camera  the Camera service object
         */
        void onError(int error, Camera camera);
    };
```


>如果说Android适配的话，Android相机方面的适配是最让人头疼的问题了。如果你的项目中有关于自定义相机方面的模块，可以说你多多少少会碰到这些异常：（1）setParamters failed（2）Camera Error 100（3）startPreView() failed（4）getParamters failed（5）拍出的照片方向倒转了（6）拍出的照片左右反即镜面效果。在这只列出

>目前所能记忆起的一些常见异常，后期如果有后续发现 ，此篇博文会继续更新，主要收集相机模块的常见异常与解决方法。

>首先说说为什么相机这块的适配很麻烦，因为Android的机型过多，而且很多厂商在Rom的时候对相机这块有修改。记得第一次遇到相机的适配问题是集成二维码扫描的功能，在魅族X4的手机上，直接相机不能预览也就是不能捕获数据，后来是把预览帧率的代码给注释了，然后就正常了。

```java
    /**
     * Media server died. In this case, the application must release the
     * Camera object and instantiate a new one.
     * @see Camera.ErrorCallback
     */
    public static final int CAMERA_ERROR_SERVER_DIED = 100;
```


>以上是Android SDK 对于Camera Error 100的描述，大概意思我相信大家都能看懂，就是要release并且重新初始化一个Camera对象，然而我在采用此种方法的时候，并不能很好的解决问题。关于此种错误我也Google了好久也在stackoverflow上看了很多帖子，每个人解决该问题的方式不一样，确切的说是针对不同的机型出现Camera Error 100的原因不同。

>下面我列举几个例子以及解决方式：

>（1）机型：HUAWEI GRA-CL00。

>原因：设置了错误的VideoSize，该手机的getSupportedVideoSizes（）方法返回为null。

>解决方法：注释掉MediaRecorder的setVideoSize()方法。

>（2）机型：Lenovo S810t

>原因：设置的VideoEncoder不支持。

>解决方法：采用默认的值，即.setVideoEncoder(MediaRecorder.VideoEncoder.DEFAULT);

>（3）机型：HTC Z715e和AMOI N820

>原因：4.0.x一个已知的问题，一开始录像会报：freeAllBuffersExceptCurrentLocked called but mQueue is not empty。这个暂时没有解决掉，但是大家可以看下下面一句话：

>This is a known problem with Android 4.0.x and it doesn't look like there is an easy fix for it.

## Reference

 -[Android相机开发中遇到的一些问题](http://www.voidcn.com/article/p-qvlqlcdy-bpn.html)
 
