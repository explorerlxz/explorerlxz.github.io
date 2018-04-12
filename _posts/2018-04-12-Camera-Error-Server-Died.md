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
