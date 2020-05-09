# kivydev64 v5.0

----------

- 已经完成  
    - [x] 采用buildozer 
    - [x] 编译安装了python37 
    - [x] kivy升级到最新稳定版  
    - [x] ndk升级到r19c 
    - [x] jnius、matplotlib、numpy打包测试通过 



这次5.0升级采用脚本升级法，在kivydev64 v2.0虚拟机上使用kdpp直接升级，升级之前需要下载一些文件放到指定目录，并且安装kdpp。

**注意** 以下所有文件解压请在虚拟机上进行，不要在windows下解压，在虚拟机可以用unzip命令解压也可以直接双击打开压缩包然后点extract解压。

需要下载的文件地址 https://cloud.189.cn/t/Q732eyRFV7zm (访问码:5dhz)

ndk下载地址 https://dl.google.com/android/repository/android-ndk-r19c-linux-x86_64.zip?hl=zh_cn

将ndk、ant-1.9.4、python37、kivymd解压到/home/kivydev/andr下

buildozer解压到/home/kivydev/test下

kdpp放到/home/kivydev/andr下运行命令

`pip install kdpp-2.0-py2-none-any.whl`

### 0x01 升级前的一些建议

如果你的网络足够好，那么就跳过这一部分内容。

#### 配置pip源

新建～/.pip/pip.conf，内容如下：

```
[global] 
index-url=http://pypi.douban.com/simple 
[install] 
trusted-host=pypi.douban.com
```

#### 配置代理

网卡设置为nat或者桥接，宿主代理打开接受局域网连接，记录ipconfig下的ipv4地址，和代理端口。在虚拟机运行下面命令，打开代理。

`sudo gedit /etc/wgetrc`

修改如下内容：

```
https_proxy = http://宿主ip:端口号/ 
http_proxy = http://宿主ip:端口号/ 
ftp_proxy = http://宿主ip:端口号/ 
use_proxy = on 
```

apt源可以通过新立得调整为阿里云或者其他国内源。


### 0x02 升级

在~/andr下运行命令，命令运行过程中会需要输入几次回车：

`kdpp runplug kivydev-upgrade`

在运行出现下面输出时直接关闭终端，再次运行上面的命令即可。

![imga](https://raw.githubusercontent.com/nkiiiiid/kivy-apk/master/imga.png)


## ======升级完毕后重启虚拟机======

默认打包python3代码，python2没有测试，可以在buildozer.spec中修改。在项目目录下运行打包命令：

`kdpp go`

等同于

`buildozer android debug`

第一次打包使用kdpp go生成我修改过的spec文件，之后可以用buildozer命令，等价的。打包的代码如果包含jnius或者其他第三方库，请修改spec文件添加对应第三方库到requirement，再打包。

在打包一个项目后，要打包其他项目时，先运行清理环境命令:

`kdpp clean`

等同于

`buildozer android clean`

打包release apk:

`kdpp release`

等同于

`buildozer android release`


### 0x03 spec文件说明

buildozer打包依靠buildozer.spec配置参数。下面简要说明下主要参数：

```
[app]

# (str) app应用名
title = My Application

# (str) 包名结尾
package.name = myapp

# (str) 包名开头，如果打包release apk必须把test改成其他任意字符串
package.domain = org.test

# (str) main.py所在目录，默认是spec所在目录
source.dir = .

# (list) 打包进apk的文件
source.include_exts = py,png,jpg,kv,atlas

# (list) 打包进apk的文件
#source.include_patterns = assets/*,images/*.png

# (list) 不打包进apk的文件
#source.exclude_exts = spec

# (list) 不打包进apk的目录，默认是tests和bin
#source.exclude_dirs = tests, bin

# (list) 不打包进apk的文件
#source.exclude_patterns = license,images/*/*.jpg

# (str) app版本
version = 0.1

# (str) Application versioning (method 2)
# version.regex = __version__ = ['"](.*)['"]
# version.filename = %(source.dir)s/main.py

# (list) 通称 requirements，打包第三方库时必须添加到这里
# comma separated e.g. requirements = sqlite3,kivy
requirements = python3,kivy

# (str) 第三方库源代码所在目录，如果指定了，打包时就会直接使用这个源代码，不会再去下载。
# Sets custom source for any requirements with recipes
# requirements.source.kivy = ../../kivy


# (list) Garden requirements，如果你用到kivy.graden的库就需要添加到这里
#garden_requirements =

# (str) app启动时的画面图片（闪屏）路径，默认是kivy图标
#presplash.filename = %(source.dir)s/data/presplash.png

# (str) app图标路径
#icon.filename = %(source.dir)s/data/icon.png

# (str) app显示方向，默认是竖屏 (one of landscape, sensorLandscape, portrait or all)
orientation = portrait

# (list) service服务列表
#services = NAME:ENTRYPOINT_TO_PY,NAME2:ENTRYPOINT2_TO_PY

#
# OSX Specific
#

#
# author = © Copyright Info

# change the major version of python used by the app
osx.python_version = 3

# Kivy version to use
osx.kivy_version = 1.9.1

#
# Android specific
#

# (bool) app是否全屏，默认否
fullscreen = 0

# (string) 闪屏的背景色
# Supported formats are: #RRGGBB #AARRGGBB or one of the following names:
# red, blue, green, black, white, gray, cyan, magenta, yellow, lightgray,
# darkgray, grey, lightgrey, darkgrey, aqua, fuchsia, lime, maroon, navy,
# olive, purple, silver, teal.
#android.presplash_color = #FFFFFF

# (list) Permissions app权限配置
#android.permissions = INTERNET

# (int) Target Android API, 目标api level
android.api = 27

# (int) Minimum API ，app能支持的最低api level
android.minapi = 21

# (int) Android SDK 版本
#android.sdk = 27

# (str) Android NDK 版本
android.ndk = 19c

# (int) Android NDK API to use. This is the minimum API your app will support, it should usually match android.minapi.
android.ndk_api = 21

# (bool) Use --private data storage (True) or --dir public storage (False)
#android.private_storage = True

# (str) Android NDK 路径
android.ndk_path = /home/kivydev/andr/android-ndk-r19c

# (str) Android SDK 路径
android.sdk_path = /home/kivydev/andr/android-sdk-linux

# (str) ANT directory 路径
android.ant_path = /home/kivydev/andr/apache-ant-1.9.4

# (bool) 是否跳过Android sdk 升级，默认否
# android.skip_update = False

# (bool) 升级sdk过程中会询问同意接受sdk license，默认是提示你选择，改为True则是默认同意接受
# android.accept_sdk_license = False

# (str) app 入口类，一般不修改
#android.entrypoint = org.renpy.android.PythonActivity

# (str) app主题类型，一般不修改
# android.apptheme = "@android:style/Theme.NoTitleBar"

# (list) 白名单
#android.whitelist =

# (str) 白名单文件路径
#android.whitelist_src =

# (str) 黑名单文件路径
#android.blacklist_src =

# (list) 自定义jar添加到这里，供jnius调用，支持通配符
# OUYA-ODK/libs/*.jar
#android.add_jars = foo.jar,bar.jar,path/to/more/*.jar

# (list) 自定义java文件添加到这里
#android.add_src =

# (list) aar文件添加到这里
#android.add_aars =

# (list) Gradle dependencies to add (currently works only with sdl2_gradle
# bootstrap)
#android.gradle_dependencies =

# (list) add java compile options
# this can for example be necessary when importing certain java libraries using the 'android.gradle_dependencies' option
# see https://developer.android.com/studio/write/java8-support for further information
# android.add_compile_options = "sourceCompatibility = 1.8", "targetCompatibility = 1.8"

# (list) Gradle repositories to add {can be necessary for some android.gradle_dependencies}
# please enclose in double quotes 
# e.g. android.gradle_repositories = "maven { url 'https://kotlin.bintray.com/ktor' }"
#android.add_gradle_repositories =

# (list) packaging options to add 
# see https://google.github.io/android-gradle-dsl/current/com.android.build.gradle.internal.dsl.PackagingOptions.html
# can be necessary to solve conflicts in gradle_dependencies
# please enclose in double quotes 
# e.g. android.add_packaging_options = "exclude 'META-INF/common.kotlin_module'", "exclude 'META-INF/*.kotlin_module'"
#android.add_gradle_repositories =

# (list) 增加java activity到manifest
#android.add_activites = com.example.ExampleActivity

# (str) OUYA Console category. Should be one of GAME or APP
# If you leave this blank, OUYA support will not be enabled
#android.ouya.category = GAME

# (str) Filename of OUYA Console icon. It must be a 732x412 png image.
#android.ouya.icon.filename = %(source.dir)s/data/ouya_icon.png

# (str) XML file to include as an intent filters in <activity> tag
#android.manifest.intent_filters =

# (str) launchMode to set for the main activity
#android.manifest.launch_mode = standard

# (list) Android additional libraries to copy into libs/armeabi
#android.add_libs_armeabi = libs/android/*.so
#android.add_libs_armeabi_v7a = libs/android-v7/*.so
#android.add_libs_arm64_v8a = libs/android-v8/*.so
#android.add_libs_x86 = libs/android-x86/*.so
#android.add_libs_mips = libs/android-mips/*.so

# (bool) Indicate whether the screen should stay on
# Don't forget to add the WAKE_LOCK permission if you set this to True
#android.wakelock = False

# (list) Android application meta-data to set (key=value format)
#android.meta_data =

# (list) Android library project to add (will be added in the
# project.properties automatically.)
#android.library_references =

# (list) Android shared libraries which will be added to AndroidManifest.xml using <uses-library> tag
#android.uses_library =

# (str) Android logcat filters to use
#android.logcat_filters = *:S python:D

# (bool) Copy library instead of making a libpymodules.so
#android.copy_libs = 1

# (str) app架构，支持: armeabi-v7a, arm64-v8a, x86, x86_64
android.arch = armeabi-v7a

#
# Python for android (p4a) specific
#

# (str) python-for-android fork to use, defaults to upstream (kivy)
#p4a.fork = kivy

# (str) 使用的p4a分支
#p4a.branch = master

# (str) p4a源代码路径，默认从网络下载
#p4a.source_dir = 

# (str) recipes路径
#p4a.local_recipes =

# (str) Filename to the hook for p4a
#p4a.hook =

# (str) Bootstrap to use for android builds
# p4a.bootstrap = sdl2

# (int) port number to specify an explicit --port= p4a argument (eg for bootstrap flask)
#p4a.port =


#
# iOS specific
#

# (str) Path to a custom kivy-ios folder
#ios.kivy_ios_dir = ../kivy-ios
# Alternately, specify the URL and branch of a git checkout:
ios.kivy_ios_url = https://github.com/kivy/kivy-ios
ios.kivy_ios_branch = master

# Another platform dependency: ios-deploy
# Uncomment to use a custom checkout
#ios.ios_deploy_dir = ../ios_deploy
# Or specify URL and branch
ios.ios_deploy_url = https://github.com/phonegap/ios-deploy
ios.ios_deploy_branch = 1.7.0

# (str) Name of the certificate to use for signing the debug version
# Get a list of available identities: buildozer ios list_identities
#ios.codesign.debug = "iPhone Developer: <lastname> <firstname> (<hexstring>)"

# (str) Name of the certificate to use for signing the release version
#ios.codesign.release = %(ios.codesign.debug)s


[buildozer]

# (int) Log level (0 = error only, 1 = info, 2 = debug (with command output))
log_level = 2

# (int) Display warning if buildozer is run as root (0 = False, 1 = True)
warn_on_root = 1

# (str) .buildozer文件路径
build_dir = /home/kivydev/test/.buildozer

# (str) Path to build output (i.e. .apk, .ipa) storage
# bin_dir = ./bin

#    -----------------------------------------------------------------------------
#    List as sections
#
#    You can define all the "list" as [section:key].
#    Each line will be considered as a option to the list.
#    Let's take [app] / source.exclude_patterns.
#    Instead of doing:
#
#[app]
#source.exclude_patterns = license,data/audio/*.wav,data/images/original/*
#
#    This can be translated into:
#
#[app:source.exclude_patterns]
#license
#data/audio/*.wav
#data/images/original/*
#


#    -----------------------------------------------------------------------------
#    Profiles
#
#    You can extend section / key with a profile
#    For example, you want to deploy a demo version of your application without
#    HD content. You could first change the title to add "(demo)" in the name
#    and extend the excluded directories to remove the HD content.
#
#[app@demo]
#title = My Application (demo)
#
#[app:source.exclude_patterns@demo]
#images/hd/*
#
#    Then, invoke the command line with the "demo" profile:
#
#buildozer --profile demo android debug

```



#### 结语

经过一段时间摸索和群里众多大佬帮助才完成了这次升级，选择buildozer是因为现在的buildozer已经解决了之前的bug，并且buildozer配置文件提供了更为便捷的配置参数。kdpp工具是去年校长大佬建议开发的，现在这个工具已经可以正式提供升级服务，虽然还有一些bug，但是我相信在后续的迭代中会不断完善和改进。

各位在使用中遇到问题可以直接在issue提出，或者到kivy中国开发者群（群号：**534622543**）向我反馈。

#### 致谢

特别感谢LR大佬（@liyuanrui）提供的最初打包通过的虚拟机。






# kivydev64 v2.0

----------

#### 打包环境下载地址 https://pan.baidu.com/s/1K5SRPxMX2iZIaMZcUWpt3w

Vbox4.3.12下载地址 https://pan.baidu.com/s/1c2Ol81E

vbox4.3.12 Extension pack下载地址 https://pan.baidu.com/s/1hsspuIC

系统 Ubuntu 16.04 64位 

用户名 kivydev 密码 kivydev 

用户名root  密码  root  

已安装增强工具，支持共享文件夹、分辨率调整、宿主机与虚拟机复制粘贴

![](https://i.imgur.com/3xJTNCJ.png)


## 前言

很高兴今天发布全新一代的apk打包环境，经过两周的摸索基本完成了1.0版虚拟机的全面升级。下一步我还将按照计划推进一些bug的修复和疑难问题的解决。今天发布的虚拟机可以正常打包apk，并且支持target sdk version 26以上打包.各位在使用中遇到问题可以直接在issue提出，或者到kivy中国开发者群（群号：**534622543**）向我反馈。

在此，感谢一如既往帮助我，支持我的朋友、大佬们。  
特别感谢校长大佬@linuxrootok


## Goals

- 已经完成  
    - [x] p4a升级到master分支，消除No such file or directory: 'src/main/assets/private.mp3' bug
    - [x] kivy升级到master分支，在linux上支持sdl2作为provider运行代码  
    - [x] ndk升级到r16b，支持api level 27打包
    - [x] sdk build tools升级到28，支持gradle编译apk  


- 未完成  
    - [ ] 增加recipe
    - [ ] jnius例子  


## 使用说明

### 0x01 为什么是p4a不是buildozer  

buildozer提供了很多自动化的操作，但是在api level 27打包中发生目前没有解决的错误，具体是gradle编译错误，错误回溯如下（我只截取关键部分），这是打包报错后，在.buildozer/android/platform/build/dists/myapp目录下运行下面命令生成的：  
`./gradlew assembleDebug  --stacktrace --info`  

错误回溯：  
```
file or directory '/home/kivydev/builddir/.buildozer/android/platform/build/dists/myapp/src/debug/java', not found
Compiling with JDK Java compiler API.
/home/kivydev/builddir/.buildozer/android/platform/build/dists/myapp/src/main/java/org/kivy/android/PythonService.java:97: error: cannot find symbol
        notification.setLatestEventInfo(context, serviceTitle, serviceDescription, pIntent);
                    ^
  symbol:   method setLatestEventInfo(Context,String,String,PendingIntent)
  location: variable notification of type Notification
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
1 error
:compileDebugJavaWithJavac FAILED
:compileDebugJavaWithJavac (Thread[Daemon worker Thread 11,5,main]) completed. Took 0.417 secs.
```

### 0x02 打包说明

虚拟机配置和v1.0是一样的，p4a配置文件如下：  
```
--dist_name py2dist
--android_api 27
--minsdk 19
--sdk_dir /home/kivydev/andr/android-sdk-linux
--ndk_dir /home/kivydev/andr/android-ndk-r10e
--ndk_version 16
--arch armeabi-v7a
--requirements python2,kivy
--private . 
--package com.myapp.test
--name py2apk
--version 1.0 
--bootstrap sdl2 
```
各参数的具体含义可以参考https://github.com/nkiiiiid/kivydev-note  
对几个参数做下说明：
- --android_api 
    打包使用的sdk version，也就是对应到apk的target api level，为了符合谷歌提出的api level>26的商店上架要求，这里设置为27.

- --minsdk
    最小sdk version，决定了apk兼容的最小安卓版本

- --arch
    目标平台的cpu架构，包括下面几种：
    - armeabi-v7a: 第7代及以上的 ARM 处理器。2011年15月以后的生产的大部分Android设备都使用它。
    - arm64-v8a: 第8代、64位ARM处理器，例如：三星 Galaxy S6。
    - armeabi: **默认值**，第5代、第6代的ARM处理器，早期的手机用的比较多。
    - x86: 平板、模拟器用得比较多。

打包命令：

`p4a apk`

打包前，先删除/home/kivydev/.local/share/python-for-android/下的builds和dists，先用sdk 19（android_api为19）打包后再删除dist保留sdk 19的build，再用sdk 27打包，这样apk target api就是 27，并且可以在安卓4.4以上正常运行。并且这个步骤对于同一个app只要执行一次，以后打包这个app的任何其他sdk版本，都只要直接修改android_api为目标版本然后打包即可，不需要先用sdk 19打包。


### 0x03 调试方法 
 
#### 有线调试vs无线调试
无论有线还是无线都需要一根usb数据线。  

- 有线调试  
打开手机开发者模式、USB调试。将手机与电脑usb连接，通过电脑上的adb命令就可以获取apk的日志。adb来自[android sdk](http://www.androiddevtools.cn/)的platform tools目录。

    - for win  
    可以下载[adbkit](http://adbshell.com/downloads)替代。在cmd下运行下面命令获取日志（假设appname为myapp）：
    `adb logcat | find /i "myapp"`

    - for ubuntu  
    通过`apt-get install android-tools-adb`安装adb。在终端运行下面命令获取日志： 
    `adb logcat | grep –i appname`

- 无线调试（手机与电脑必须在同一局域网）
打开手机wifi，手机设置同上，手机通过usb连接电脑后，运行如下命令，打开手机5555端口用于通讯

    `adb tcpip 5555`

    然后拔掉usb，使用下面命令连接手机ip地址

    `adb connect 192.168.2.102`

    使用adb安装apk

    `adb install -r apk路径`



#### 关于日志  
logcat返回的日志信息较少，不够直观，如果apk有运行到loading界面（也就是闪屏）之后的步骤，那么在apk安装目录（/data/data/package name/files/app/.kivy/logs）下会生成log文件，这个文件包含了详细的apk运行过程，对于查找闪退原因等问题帮助很大。以ubuntu平台为例，在终端运行下面命令获取log文件内容（假设package name为com.myapp.test）：
```
adb shell       #进入shell
run-as com.myapp.test
cd /data/data/com.myapp.test/files/app/.kivy/logs
ls              #可以看到运行日志
```

但是某些手机（比如三星）对run-as做了限制不能使用，只能用adb backup备份命令来备份apk的目录，然后使用dd命令（linux命令，对于win来说需要安装cygwin才有该命令，推荐安装MobaXterm，创建本地session会提示下载cygwin插件，装上插件后就可以使用dd命令，MobaXterm是集ssh、telnet、rsh、rdp、vnc、ftp、xdmcp等等功能于一体的工具）去掉头部，使用python的zlib解压后得到tar包。命令如下：
```
adb backup -f data.ab com.myapp.test

dd if=data.ab bs=1 skip=24 | python -c "import zlib,sys;sys.stdout.write(zlib.decompress(sys.stdin.read()))" > data.tar
```
或者zlib解压后直接解压

`dd if=data.ab bs=1 skip=24 | python -c "import zlib,sys;sys.stdout.write(zlib.decompress(sys.stdin.read()))" | tar -xvf -`

也可以使用openssl的zlib解压，不过我的ubuntu openssl并没有支持zlib。 

`dd if=data.ab  bs=1 skip=24 | openssl zlib -d > data.tar`


**adb backup非常锋利，是个大杀器，可以备份所有apk的安装目录，如果apk的账号密码是明文保存，那么一旦获取到安装目录也就获取了apk中的账号密码，一些充电站要求打开手机开发者模式和usb调试基本是用这个套路获取用户账号信息的。**


### 0x04 如何自己修改打包环境  

说明下打包有关配置含义。
apk打包需要sdk、ndk、ant，其中[sdk、ndk](http://www.androiddevtools.cn/)下载后放在/home/kivydev/andr，ant是通过synaptic安装的，位于/usr/share。

#### sdk  
sdk版本（即api level）和ndk版本通常要同步上调才能保证打包成功。具体是你所使用的sdk version也就是sdk\platform下的目标版本必须在ndk\platform有对应的版本。  
比如kivydev64 v1.0使用的版本对应是：  
sdk19，ndk10e。这是对应安卓4.4.2平台，在更高版本的安卓手机可能不能运行。  
现在v2.0使用的版本是  
sdk27，ndkr16b。这是对应安卓8.1平台。

p4a配置文件的android_api就是sdk version，在编译脚本中将该参数同时作为compile sdk version和target sdk version。

#### api level与android版本对应图  
![](https://i.imgur.com/UPMS9SM.png)


#### p4a结构  
p4a位于/home/kivydev/.local/lib/python2.7/site-packages/  
p4a编译生成的build、dists位于/home/kivydev/.local/share/python-for-android/  

清除builds、dists命令：  

`p4a clean_builds && p4a clean_dists`





### 0x05 Q&A

Q:道理我都懂，可是打包环境怎么这么大（越来越大）？  
A:可能它觉得大点比较有存在感:smile:

Q:万事俱备，就差怎么入门kivy？  
A:kivy的书籍不多，可以看这本入门kivy-interactive-applications-in-python，它关于界面的写法习惯非常值得借鉴。

Q:是否有打算写个kivy教程？  
A:我的手有自己的想法，哪天我问问。

Q:kivy有商业应用吗？  
A:目前没看到，你可以做第一个。

Q:kivy太冷门，不如用原生。  
A:见仁见智，兴趣使然。

final，能帮助萌新降低学习门槛就足够了。  

#### **大佬们救救孩子吧，现在可以捐助作者了，捐助我有机会看到更多教程。**

<img src="https://i.imgur.com/94DkJbs.jpg" width = "600" height = "350" alt="捐助up" align=center />


# kivydev64 v1.0

![](https://img.shields.io/badge/ubuntu-16.04-orange.svg) ![](https://img.shields.io/badge/xfce-v4-orange.svg)

![](https://img.shields.io/badge/kivy-1.10.1-brightgreen.svg) ![](https://img.shields.io/badge/python-2.7-brightgreen.svg) ![](https://img.shields.io/badge/python-3.5-brightgreen.svg) ![](https://img.shields.io/badge/pythonforandroid-0.5-brightgreen.svg) ![](https://img.shields.io/badge/build-passing-brightgreen.svg)

## 写在前面

kivy打包有两种工具，分别是p4a和buildozer，kivydev64使用p4a，kivydev使用buildozer。
buildozer其实是对p4a做了进一步封装，换汤不换药。如果你不想配置recipe和dist之类的参数，可以使用buildozer，<del>但是每次都要复制已经打包成功的项目目录下的.buildozer到要打包的项目目录下，buildozer才不会重复下载sdk和ndk等。而.buildozer目录通常在1G以上，每个项目目录如果都复制一份，不久就会耗尽虚拟机的硬盘空间。所以推荐使用p4a，也就是kivydev64.</del>  buildozer.spec文件的build_dir参数可以设定.buildozer的路径。

`build_dir = /root/.buildozer`

这个打包环境是第一个建立在64位ubuntu的环境。


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

#### 关于apk兼容性(已经在kivydev64v2.0解决）

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
注释掉ln16，默认包含项目目录下的所有文件。  
ln39，增加python2，没有这项打包时会出现找不到hostPython错误。  
ln180，改为2  
ln183，改为0  

```
buildozer android_new debug     #生成debug版本apk

buildozer android_new release   #生成release版本apk
```

如果有其他错误或者完善建议欢迎issue
