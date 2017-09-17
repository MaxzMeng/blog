---
layout:     post
title:      "一步步带你实现Android网络状态监听"
subtitle:   " 我的第一篇Android文章"
date:       2017-09-01 12:47:00
author:     "Max"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
---
#### 前言
最近又在重新看《第一行代码》这本书，在第五章中有一个样例，可以检测当前网络状态，但是只能判断当前网络是否可用，在此之上我想是否能做出一个和我们日常使用的APP一样判断当前网络是wifi还是移动网络的样例出来，于是便有了这篇博文的诞生，同样这也是我第一次写博文，希望能够给需要的人带来一些启发。
#### 检测网络变化
+ 首先在清单文件里加入权限
``` <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />```
这里我们加入的这个权限并非```<uses-permission android:name="android.permission.INTERNET" />```
因为我们只需要应用去获取当前网络状态，而不需要去通过应用去上网，所以只加入上述一个权限即可。
+ 实现书上的样例
```java
    class NetworkChangeReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            Toast.makeText(context, "网络状态改变", Toast.LENGTH_SHORT).show();
        }
    }
```
由于代码比较少，直接在MainActivity里写一个内部类，让它继承自BroadcastReceiver，命名为NetworkChangeReceive，并重写onReceive方法，当检测到网络状态变化时弹出一个Toast。
``` java 
private IntentFilter intentFilter;
private NetworkChangeReceiver networkChangeReceiver;
```
在全局变量中增加两个变量。

```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        intentFilter = new IntentFilter();
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
        networkChangeReceiver = new NetworkChangeReceiver();
        registerReceiver(networkChangeReceiver, intentFilter);
    }
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(networkChangeReceiver);
    }
```
在onCreate方法里对networkChangeReceiver进行注册并且在onDestroy方法里进行注销。

![](http://upload-images.jianshu.io/upload_images/6524321-6ad8dcd34818fdcc.gif?imageMogr2/auto-orient/strip)
现在让我们运行程序，可以发现程序已经能够判断网络状态的变化，但是这还是不够的，所以接下来让我们接着进行改进。
#### 检测当前网络是否可用
```java
        public void onReceive(Context context, Intent intent) {
            ConnectivityManager connectivityManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            if (networkInfo != null && networkInfo.isAvailable()) {
                Toast.makeText(context, "当前网络可用", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(context, "当前网络不可用", Toast.LENGTH_SHORT).show();
            }
        }
```
 在onReceive()方法中，首先通过getSystemService()方法得到了ConnectivityManager的实例，这是一个系统服务类，专门用于管理网络连接的。然后调用它的getActiveNetworkInfo()方法可以得到NetworkInfo的实例，接着调用NetworkInfo的isAvailable()方法就可以判断出当前是否有网络了。

![](http://upload-images.jianshu.io/upload_images/6524321-8ef54cf0243f91b1.gif?imageMogr2/auto-orient/strip)
再次运行程序，发现已经可以判断当前网络是否可用，书上的例子到这里也就结束了，不过之后我们只需要对当前网络的类型进行判断就可以完成我们最终的目标了。
#### 判断当前网络属于wif还是流量

![](http://upload-images.jianshu.io/upload_images/6524321-17c62cbbabc20618.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
通过查看API，找到networkInfo中的getType方法可以返回当前网络类型。一共有五种类型，而其中的TYPE_MOBILE和TYPE_WIFI正是我们所需要的。

```java
class NetworkChangeReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            ConnectivityManager connectionManager = (ConnectivityManager) getSystemService(CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectionManager.getActiveNetworkInfo();
            if (networkInfo != null && networkInfo.isAvailable()) {
                switch (networkInfo.getType()) {
                    case TYPE_MOBILE:
                        Toast.makeText(context, "正在使用2G/3G/4G网络", Toast.LENGTH_SHORT).show();
                        break;
                    case TYPE_WIFI:
                        Toast.makeText(context, "正在使用wifi上网", Toast.LENGTH_SHORT).show();
                        break;
                    default:
                        break;
                }
            } else {
                Toast.makeText(context, "当前无网络连接", Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```

![](http://upload-images.jianshu.io/upload_images/6524321-5585ae3bab2d4740.gif?imageMogr2/auto-orient/strip)
尝试修改代码，运行。发现程序实现了我们想要的功能，至此你已经实现了判断当前网络类型的功能。