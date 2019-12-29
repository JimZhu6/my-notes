# 树莓派的其他玩法

> 记录了使用树莓派部分安装软件的笔记

## 安装百度云盘下载

项目：[BaiduPCS-Go](https://github.com/iikira/BaiduPCS-Go)

确认树莓派系统是32还是64为，运行命令  `getconf LONG_BIT`  结果为：32

确认树莓派的CPU类型：`uname -a` 结果如下：

Linux wzfpi 4.19.57-v7l+ #1244 SMP Thu Jul 4 18:48:07 BST 2019 armv7l GNU/Linux

由此确定为armv7的CPU，系统为32位，所以我需要下载的是BaiduPCS-Go-v3.6-linux-armv7.zip

下载后解压，通过vncviewer传输到桌面，即/home/pi/Desktop下

修改权限：

```sh
chmod 777 /home/pi/Desktop/BaiduPCS-Go
```

执行 `sudo /home/pi/Desktop/BaiduPCS-Go`或者`"/home/pi/Desktop/BaiduPCS-Go"`