# 树莓派笔记

> 本文部分节选自：[我的世界中文论坛-在树莓派下Minecraft服务器搭建教程](https://www.mcbbs.net/thread-579651-1-1.html)、

> 相关网站:[树莓派实验室](http://shumeipai.nxez.com/)



## 安装Raspbian操作系统

从树莓派官网处[下载官方镜像](https://www.raspberrypi.org/downloads/raspbian/)。系统需要写进SD卡，需要先用到[SDFomatter](https://www.sdcard.org/chs/downloads/formatter/index.html)格式化SD卡，然后使用[Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)对SD卡进行写盘。等待写盘完成后，需要在电脑上找到一个刚刚烧录好的磁盘分区，名字叫`boot`，在里面创建一个文件名为`SSH`的文件（**无后缀名**），这时就可以将SD卡插回树莓派了。

为树莓派接上MicroUSB电源，如果正常的话红灯将会是常亮的，绿灯将会是闪烁的，Raspbian操作系统就成功地搭载到你的树莓派了！（如果红灯会闪烁的话那么你的电源就是不合格的了）



## 连接到树莓派

### 通过路由+网线连接

使用网线将树莓派喝路由器连接起来，其他连接了路由的设备进入路由器的管理界面，查看树莓派的ip地址，然后就可以使用PuTTY或者MobaXterm通过ssh连接树莓派，默认用户名是`pi`，密码是`raspberry`。



### 通过VNCviewer连接树莓派

在使用ssh的方式连接到树莓派之后,安装VNC服务端：

```sh
sudo apt-get install tightvncserver
```

安装完成后，需要启动VNC并设置VNC连接的端口

```sh
vncserver :1      # 这里冒号后面的数字可以修改，为你通过VNC来连接的端口号
```

然后设置密码，然后就设置完成VNC的服务器端了。

> 如果你下次开机还想通过VNC来连接的话，那么就在`/etc/init.d`这一个文件中把`vncserver:1`这条指令添加进去

这时在windows端打开[VNCviewer](https://www.realvnc.com/en/connect/download/viewer/)，输入ip和端口后即可连接到树莓派的图形化界面。

这时推荐做以下设置：

![进入设置菜单](./img/sp1.jpg)

  打开左上角的`Menu`一栏，选择其中的`Preferences`一栏，再选择`Raspberry Pi Configuration`一栏，出现系统设置界面。

![设置菜单](./img/sp2.jpg)

- 点击`Expand Filesystem`，扩容SD卡至卡原本的大小
- 点击`Change Password`修改登录密码
- 更改菜单至`Performance`，将`Overclock`（超频）和`GPU Memory`（显存分配）设置改为下图：

![设置超频](./img/sp3.jpg)

> 关于显存分配，这里有几个可选值：16/32/64/128/256
>
> 如果你将你的树莓派用作文件服务器或Web服务器，不需要使用视频输出，你可以减少分配给GPU的内存数量（最少为16MB）
> 如果你用它来浏览网页，看视频甚至运行3D游戏，那么你应该为GPU分配较大的内存，从而提高GPU性能，使其更好地渲染3D游戏画面
> 如果你需要接入摄像头，则**至少**要为要为GPU分配128MB显存

设置完成后重启生效。

