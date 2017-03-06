打包环境下载地址 http://pan.baidu.com/s/1gfgeOT1

这里对这个打包apk的环境做下说明，以前那个版本的环境没有写说明结果发现确实不妥，还是交待下应该交待的东西(～￣▽￣)～

运行环境推荐vbox 4.3.12，当然其他版本的vbox也是可以的。
系统Ubuntu 16.04，装了xfce桌面，运行起来比较快，如果你的主机配置不错的话可以注销用户切换到unity。
这个环境使用buildozer进行打包，现在的buildozer已经默认使用sdl2进行打包了。

用户名 kivydev 密码 kivydev
用户名 root 密码 root

系统已经有一个测试打包的目录/home/kivydev/kivy，这个目录下的accordion是从官方的example中复制过来的，更名为main.py，注意凡是要打包的程序，入口代码所在的脚本名必须为main.py。


打包命令
在打包前必须把测试打包项目accordion目录下的.buildozer复制到你的项目目录下。

```
buildozer init
#生成buildozer.spec配置文件，这个配置文件包括各种apk的配置，这里我对原始的bulldozer.spec配置文件做了修改（原始spec文件位于/usr/local/lib/python2.7/dist-packages/buildozer/default.spec/default.spec）修改的内容有：
注释掉ln16，默认包含项目目录下的所有文件
ln39，增加python2，没有这项打包时会出现找不到hostPython错误。
ln180，改为2
ln183，改为0
```


```
buildozer android_new debug     #生成debug版本apk

buildozer android_new release   #生成release版本apk
```

如果有其他错误或者完善建议欢迎issue
