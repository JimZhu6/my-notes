# php+reids

## 安装

### Windows

> 这一段文章参考[如何在windows安装php redis扩展](http://www.xiabingbao.com/php/2017/08/27/window-php-redis.html)

#### 下载redis程序

在GitHub上下载Windows版，其中早期版本可以在[这里](https://github.com/MicrosoftArchive/redis/releases)找到（推荐安装），而[这里](https://github.com/tporadowski/redis/releases)可以找到更新的版本。

#### 获取redis扩展

由于wampserver默认没有提供redis扩展，需要自己下载，在wampserver启动后，打开本地页面http://localhost，点击左下角的`phpinfo()`查看自己的版本。主要留意这三条数据：

- php version : `7.2.10`
- Architecture : `x64`
- PHP Extension Build : `API20170718,TS,VC15`

redis扩展是有两个文件的: `php_igbinary.dll`和`php_redis.dll`。

##### 选择igbinary

在这个[链接](https://windows.php.net/downloads/pecl/releases/igbinary/)找到各个版本的文件，**根据自己的参数来选择**，php版本是7.0,TS,VC15，cpu是x64，那我们可以选择2.08里面的`php_igbinary-2.0.8-7.2-ts-vc15-x64.zip`

##### 选择redis

在这个[链接](https://windows.php.net/downloads/pecl/releases/redis/)找到各个版本的文件，同样根据自己的参数来选择下载对应的文件。

#### 安装扩展

下载了上面两个文件之后，将他们解压，获取到他们各自的`*.dll`文件，将这个文件放到`\wamp\bin\php\php7.2.10\ext`中。然后，在任务栏点击wampserver图标，指向php，选择`php.ini`，在弹出的txt编辑器里添加：

```
;redis
extension=php_igbinary.dll
extension=php_redis.dll
```

#### 测试

重启wampserver，如果在`phpinfo()`中能看到redis，则说明扩展已经安装成功了。

在Windows本地安装redis的根目录里启动服务

```shell
redis-server.exe redis.windows.conf
# 收到下面的返回则表示启动成功
[11012] 12 Feb 13:07:17.941 # Creating Server TCP listening socket 127.0.0.1:6379: bind: No error
```

在php程序中测试下面的指令

```php
$redis = new Redis();                   //redis对象
$redis->connect("127.0.0.1","6379"); //连接redis服务器
$redis->set("test","Hello World");      //set字符串值
echo $redis->get("test");               //获取值
```

如果能输出`Hello World`则说明redis能正常存取数据了。



### Linux

参考[菜鸟教程](http://www.runoob.com/redis/redis-php.html)





