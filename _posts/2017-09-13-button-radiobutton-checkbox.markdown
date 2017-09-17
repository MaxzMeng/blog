---
layout:     post
title:      "Android基础控件的简单自定义"
subtitle:   " Button,RadioButton,CheckBox的简单自定义"
date:       2017-09-13 21:00:00
author:     "Max"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
---

#### 前言
在实际开发过程中，通常我们能够直接使用Android自带的控件的机会很少，我们都会对这些基础控件进行或多或少的自定义以达到自己想要的效果，接下来我就简单介绍一下Button,RadioButton,CheckBox的简单自定义。
#### Button
在drawable下新建一个bg_button.xml文件，我们将通过这个文件来实现自定义效果。
在这里我先把写好的代码贴上来，然后逐层分析。
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true">
        <shape>
            <solid android:color="#FFFFFF" />
            <corners android:radius="5dp" />
        </shape>
    </item>
    <item android:state_pressed="false">
        <shape>
            <solid android:color="#000000" />
        </shape>
    </item>
</selector>
```
+ 首先是selector,按钮有被按压和未被按压两种状态，对于这两种不同状态的判断，就会在selector里进行判断。
+ 接下来两个同级的item子标签就分别代表按钮是否被按压。通过```android:state_pressed=""```来判断，true代表被按压，false代表没有。
+ shape里的属性就是我们最终要实现的效果。
+ solid是填充色，这里我把未按压设置为纯黑，按压设置为纯白。
+ corners可以把button的四个角设置为圆角，数值越大越圆。

所以我们最后实现了一个这样的button，在未被按压时颜色为纯黑，被按压时变为纯白，并且带有5dp的圆角。

最后，我们在布局文件需要实现这样效果的button加入```android:background="@drawable/bg_button"```。

之后运行程序。

![](http://upload-images.jianshu.io/upload_images/6524321-42a00b658a82dbff.gif?imageMogr2/auto-orient/strip)
#### RadioButton
RadioButton的自定义和Button的相似，只要定义好想要实现效果的xml文件即可，因此我在这里省去其他多余的步骤，只放上xml的代码
```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_checked="true">
        <shape>
            <solid android:color="#76EE00" />
        </shape>
    </item>
    <item android:state_checked="false">
        <shape>
            <solid android:color="#FF9900" />
        </shape>

    </item>
</selector>
```
这里需要注意的是记得给RadioButton设置一个```android:button="@null"```的属性，用来取消掉RadioButton默认的按钮。
可以看到基本上和上面的Button是一样的，只不过RadioButton的选取用的是```android:state_checked="true/false"```这个属性,之后在布局文件里设置一下，就可以实现下面的效果。

![](http://upload-images.jianshu.io/upload_images/6524321-cc8144dfcad73ead.gif?imageMogr2/auto-orient/strip)

#### CheckBox
我准备了两张图片，selected和unselected,分别对应选中和未选中状态，放在drawable文件夹下。
之后还是通过简单的定义xml文件来达到想要的效果。
```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/unselected" android:state_checked="false" />
    <item android:drawable="@drawable/selected" android:state_checked="true" />
</selector>
```

![](http://upload-images.jianshu.io/upload_images/6524321-bc3a44e138cbe969.gif?imageMogr2/auto-orient/strip)
