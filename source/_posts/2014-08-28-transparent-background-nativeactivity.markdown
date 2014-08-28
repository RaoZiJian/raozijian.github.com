---

layout: post
title: "如何在安卓上给应用程序设置透明背景 NativeActivity篇"
date: 2014-08-28
comments: true
categories: Cocos2d-x QA
tags: [安卓,Android]

---

###Question: 如何在安卓上给应用程序设置透明背景？

####1.以Cocos2d-x 3.0 rc0为例，对于NativeActivity,效果图：

![](http://cocos2d-xqa.qiniudn.com/1.png)

####就如果你想让你的activity透明，很简单，只需要4步。

* 第一步：打开AndroidManifest.xml文件，添加Translucent到	

```
android:theme="@android:style/Theme.NoTitleBar.Fullscreen">
```

![](http://cocos2d-xqa.qiniudn.com/2.png)

* 第二步：打开Cocos2dxActivity.java文件，添加

```
getWindow().setFormat(PixelFormat.TRANSLUCENT);
```
![](http://cocos2d-xqa.qiniudn.com/3.png)

* 第三步：打开cocos2d/cocos/2d/CCDirector.cpp文件，找到setGLDefaultValues()函数，修改最后一个alpha值，范围从0.0f到1.0f。 

```
glClearColor(0.0f, 0.0f, 0.0f, 1.0f);
```

![](http://cocos2d-xqa.qiniudn.com/4.png)
![](http://cocos2d-xqa.qiniudn.com/5.png)

* 第四步：打开cocos2d/cocos/2d/platform/android/nativeactivity.cpp，找到engine_init_display(struct engine* engine)函数，修改如下数组 

![](http://cocos2d-xqa.qiniudn.com/6.png)

修改

```
const EGLint attribs[] = {
            EGL_SURFACE_TYPE, EGL_WINDOW_BIT,
            EGL_RENDERABLE_TYPE, EGL_OPENGL_ES2_BIT,
            EGL_BLUE_SIZE, 5,
            EGL_GREEN_SIZE, 6,
            EGL_RED_SIZE, 5,
            EGL_DEPTH_SIZE, 16,
            EGL_STENCIL_SIZE, 8,
            EGL_NONE
    };
```

成

```
const EGLint attribs[] = {
                EGL_SURFACE_TYPE, EGL_WINDOW_BIT,
                EGL_RENDERABLE_TYPE, EGL_OPENGL_ES2_BIT,  
 //EGL_BLUE_SIZE, 5,   -->delete 
 //EGL_GREEN_SIZE, 6,  -->delete 
 //EGL_RED_SIZE, 5,    -->delete 
                EGL_BUFFER_SIZE, 32,  //-->new field
                EGL_DEPTH_SIZE, 16,
                EGL_STENCIL_SIZE, 8,
                EGL_NONE
        };
```

![](http://cocos2d-xqa.qiniudn.com/7.png)

搞定收工
	
