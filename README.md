# kivydev64 v1.0

![](https://img.shields.io/badge/ubuntu-16.04-orange.svg) ![](https://img.shields.io/badge/xfce-v4-orange.svg)

![](https://img.shields.io/badge/kivy-1.10.1-brightgreen.svg) ![](https://img.shields.io/badge/python-2.7-brightgreen.svg) ![](https://img.shields.io/badge/python-3.5-brightgreen.svg) ![](https://img.shields.io/badge/pythonforandroid-0.5-brightgreen.svg) ![](https://img.shields.io/badge/build-passing-brightgreen.svg)

## 写在前面

kivy打包有两种工具，分别是p4a和buildozer，kivydev64使用p4a，kivydev使用buildozer。
buildozer其实是对p4a做了进一步封装，换汤不换药。如果你不想配置recipe和dist之类的参数，可以使用buildozer，但是每次都要复制已经打包成功的项目目录下的.buildozer到要打包的项目目录下，buildozer才不会重复下载sdk和ndk等。而.buildozer目录通常在1G以上，每个项目目录如果都复制一份，不久就会耗尽虚拟机的硬盘空间。所以推荐使用p4a，也就是kivydev64，这个打包环境也是第一个建立在64位ubuntu的环境。


## 0x00 系统配置

打包环境下载地址 http://pan.baidu.com/s/1slweL8T

Vbox4.3.12下载地址 https://pan.baidu.com/s/1c2Ol81E

vbox4.3.12 Extension pack下载地址 https://pan.baidu.com/s/1hsspuIC

系统 Ubuntu 16.04 64位 

用户名 kivydev 密码 kivydev 

用户名root  密码  root  

已安装增强工具，支持共享文件夹、分辨率调整、宿主机与虚拟机复制粘贴


## 0x01 打包

#### 打包命令
/home/kivydev/test是测试目录，该目录下的py2apk是py27打包，py3apk是py35打包.无论是哪种版本py打包命令都是一样的,在py2apk或py3apk目录下执行：

`p4a apk`
  
不同之处在于.p4a配置文件，这里简要说明下两个版本py的配置文件。


#### py2apk的.p4a
```
--dist_name py2dist             使用py2dist，这个参数是使用已经编译好的库，如果是第一次打包那么p4a会自动创建该文件名的dist，位于/home/kivydev/.local/share/python-for-android/dists/    
--android_api 19                                   api level
--sdk_dir /home/kivydev/andr/android-sdk-linux     sdk路径
--ndk_dir /home/kivydev/andr/android-ndk-r10e      ndk路径
--requirements python2,kivy                        
--private .                       项目路径
--package com.myapp.test          包名
--name py2apk                     app名
--version 1.0                     版本号
--bootstrap sdl2                  引导
```

#### py3apk的.p4a
```
--dist_name py3dist                                 区别于py2
--android_api 19
--sdk_dir /home/kivydev/andr/android-sdk-linux
--ndk_dir /home/kivydev/andr/crystax-ndk-10.3.2   区别于py2
--requirements python3crystax,kivy           区别于py2
--private .
--package com.myapp.py3apk
--name py3apk
--version 1.0 
--bootstrap sdl2 
```

p4a详细的参数参见pythonforandroid官方文档，我上传了一份到本repo。


## 0x02 py35运行代码

```
python3  main.py
```


## 0x03 更新日志

* 使用p4a取代buildozer

* 支持py35打包apk

* 支持py35本地运行kivy。


## 0x04 说明

#### 关于apk兼容性

之前的buildozer打包出来的apk有的不能在安卓6、7的某些机型上面运行，要解决这个问题还需要大量测试，江湖传说API level 19兼容最好，所以我就用了19，有安卓6和7手机的可以打包试试看，如果不能运行务必反馈给我机型和安卓系统版本。

#### 关于虚拟机导入失败问题

有用户反映说导入虚拟机失败，各种提示都有，至于原因嘛，其实只有一个那就是不够帅，只要是颜值9分以上的都可以成功导入，如果是丑的人那就找一个帅的人帮助，或者在每天早晨8点准时导入才能成功（已经由校长巨佬权威测试通过）。

<img src="http://i.imgur.com/Q3mXJkB.png" width = "200" height = "180" alt="萌新三连" align=center />

#### 鸣谢

这个打包工具的更新得到了@校长叫我起床 巨佬的大力帮助，在此表示感谢。另外这个工具的更新意见来自目前国内最大最强的kivy中文开发者交流群（**534622543**）众多群友。欢迎大家使用并将发现的bug和不足告诉我，我会尽量修复。打包环境搭建是个苦差事，谈不上什么技术含量，但是需要足够的耐心和灵光一现的运气。如果你想独自搭建环境，我建议你先通读下PythonforAndroid文档，对每个步骤都反复阅读几遍，不要在搭建过程中遗漏步骤，发现问题先谷歌，谷歌不到的就择日再搭建。

<img src="http://i.imgur.com/CY76Gaj.jpg" width = "300" height = "150" alt="萌新三连" align=center />

最后预祝大家打包愉悦(<ゝω·)☆~Kira~

欢迎大家加入kivy中文开发者交流群 群号：534622543

<img src="https://i.imgur.com/ptehAit.jpg" width = "300" height = "400" alt="群号：534622543" align=center />


## 0x05 旧版kivydev

打包环境下载地址 http://pan.baidu.com/s/1gfgeOT1

Vbox4.3.12下载地址 https://pan.baidu.com/s/1c2Ol81E

vbox4.3.12 Extension pack下载地址 https://pan.baidu.com/s/1hsspuIC

这里对这个打包apk的环境做下说明，以前那个版本的环境没有写说明结果发现确实不妥，还是交待下应该交待的东西(～￣▽￣)～

运行环境推荐vbox 4.3.12，当然其他版本的vbox也是可以的。
系统Ubuntu 16.04，装了xfce桌面，运行起来比较快，如果你的主机配置不错的话可以注销用户切换到unity。
打包环境已经包含了增强工具，支持共享文件夹，调整分辨率。
这个环境使用buildozer进行打包，现在的buildozer已经默认使用sdl2进行打包了。

用户名 kivydev 密码 kivydev
用户名 root 密码 root

系统已经有一个测试打包的目录/home/kivydev/kivy，这个目录下的accordion是从官方的example中复制过来的，更名为main.py，注意凡是要打包的程序，入口代码所在的脚本名必须为main.py。


打包命令
在打包前必须把测试打包项目accordion目录下的.buildozer复制到你的项目目录下。

```
buildozer init      #生成buildozer.spec配置文件，这个配置文件包括各种apk的配置
```
这里我对原始的bulldozer.spec配置文件做了修改（原始spec文件位于/usr/local/lib/python2.7/dist-packages/buildozer/default.spec/default.spec）修改的内容有：
注释掉ln16，默认包含项目目录下的所有文件
ln39，增加python2，没有这项打包时会出现找不到hostPython错误。
ln180，改为2
ln183，改为0

```
buildozer android_new debug     #生成debug版本apk

buildozer android_new release   #生成release版本apk
```

如果有其他错误或者完善建议欢迎issue
