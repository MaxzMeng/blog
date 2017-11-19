---
layout:     post
title:      "Linux下Android开发环境的搭建"
subtitle:   ""
date:       2017-11-19 12:00:00
author:     "Max"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
    - Linux
---

### java环境的配置
本文默认读者已安装好java环境，如果没有请按照下面的链接里的步骤来配置。
http://www.yiibai.com/java/how-to-install-java-on-ubuntu.html
## 安装AndroidStudio
### 通过命令行自动安装
在终端输入```sudo apt-get install android-studio```然后输入密码就会自动帮你安装android并帮你配置好所有的环境变量，而且还会把androidstudio自动加入到启动器中，可以说是不能再方便，但是这样做有几点坏处:
+ 下载速度很慢，只有几百k 
+ 如下图所示，除了必要的AndroidStudio，还会为我们自动安装openjdk等许多不必要的东西。
![](http://upload-images.jianshu.io/upload_images/6524321-639e97bf084b5d4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 在写本文的时候，AndroidStudio已经更新到3.0版本，而通过终端下载的版本还是2.3.3，安装之后需要自己再手动更新到3.0，比较麻烦
### 手动安装
+ 到[官方网站](https://developer.android.google.cn/studio/index.html)下载AndroidStudio for linux的安装包。
+ 如果你运行的是64位的系统，你需要在终端里输入```sudo apt-get install lib32z1 lib32ncurses5 lib32stdc++6```来安装32位的兼容库。
+ 把下载的压缩包解压到你想要的位置。
+ cd进解压好的文件夹的bin目录，在终端里输入```./studio.sh```,AndroidStudio开始运行，至此AndroidStudio基本安装完成。
### 添加到启动器
如果是自己手动安装的AndroidStudio，系统不会为你自动创建类似于windows下的快捷方式，需要自己去手动创建
![](http://upload-images.jianshu.io/upload_images/6524321-abe15535e587cd85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点击AndroidStudio欢迎界面下的Configure下的Create Desktop Entry就可以了。
### 添加Android和adb的环境变量
在终端里输入```sudo su``` 获取管理员权限，之后```gedit /etc/profile```  在末尾配置你的Android环境变量
添加如下两行
```
export PATH=/你的Android SDK安装路径/tools:$PATH
export PATH=/你的Android SDK安装路径/platform-tools:$PATH
```
下图是我的路径，给大家做个参考
![](http://upload-images.jianshu.io/upload_images/6524321-d80bc227418959a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
添加完成后保存，重启电脑或者在终端中输入```source /etc/profile```来使刚更改的环境变量生效。
###测试环境变量
分别在终端中输入adb和android，如果能看到包含下面两个图中的内容就说明环境变量配置成功。
![](http://upload-images.jianshu.io/upload_images/6524321-2036ab60d916deff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/6524321-4bcd2c8375d004e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




至此，AndroidStudio已经安装完成，下边我们还要解决一些其他的小问题
### 添加字体
consolas是我在windows下最喜欢用的字体，但是在我的Deepin上并没有内置这种字体，所以我就以consolas为例演示如何添加别的字体。
首先要准备好你想要添加字体的.ttf文件，有的linux发行版能够直接打开进行安装，就像下图这样![](http://upload-images.jianshu.io/upload_images/6524321-c488115b6b753f5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我只需要点击安装然后重启电脑就能在AndroidStudio中愉快的使用刚刚添加的字体了。

如果不能进行这样的操作，我们也可以通过终端来完成。
```
sudo mkdir -p /usr/share/fonts/vista
sudo cp 字体文件名称.ttf /usr/share/fonts/vista/
sudo chmod 644 /usr/share/fonts/vista/*.ttf
cd /usr/share/fonts/vista/
sudo mkfontscale
sudo mkfontdir
sudo fc-cache -fv
```
之后我们打开AndroidStudio,把Show only momospaced fonts的勾选取消，就可以找到刚刚添加的字体了。
![](http://upload-images.jianshu.io/upload_images/6524321-d427f9d8d653c7ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
###科学上网
linux下并没有有图形界面的ssr客户端，但是有一款ss客户端是有图形化界面的。这款软件需要我们搭配一款名为	SwitchyOmega的Chrome插件来使用。
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```
之后打开就可以添加你自己的节点了。
![](http://upload-images.jianshu.io/upload_images/6524321-b8d831f9483ba77d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
添加完成后，我们去SwitchyOmega进行设置。
首先按照下图把代理协议改为SOCKS5,代理服务器改为127.0.0.1,代理端口改为1080
![](http://upload-images.jianshu.io/upload_images/6524321-eb4f1f13c7bcfe46.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后点击添加规则列表，选择AutoProxy,把```https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt```复制进去点击立即更新情景模式。
![](http://upload-images.jianshu.io/upload_images/6524321-a884c164b9615c20.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后在autoswitch里把我们刚才添加的规则选择为用代理运行，默认直连。保存。
![](http://upload-images.jianshu.io/upload_images/6524321-14820b6df853ccfd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
之后把插件切换到auto switch模式，你就可以科学上网了。
![](http://upload-images.jianshu.io/upload_images/6524321-03ed76305faee909.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)











