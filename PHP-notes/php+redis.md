# php+redis

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
$redis->connect("127.0.0.1","6379");    //连接redis服务器
$redis->set("test","Hello World");      //set字符串值
echo $redis->get("test");               //获取值
```

如果能输出`Hello World`则说明redis能正常存取数据了。



### Linux

参考[菜鸟教程](http://www.runoob.com/redis/redis-php.html)



## [redis配置](http://www.redis.net.cn/tutorial/3504.html)

在redis安装目录下的名为`redis.windows.conf`与`redis.windows-service.conf`分别对应客户端和服务器端的配置项。

可以通过`redis-cli.exe`客户端输入命令的方式查看与更改。

```shell
# 使用*获取所有配置项
redis 127.0.0.1:6379> CONFIG GET *
# 或者指定获取某一个配置项
redis 127.0.0.1:6379> CONFIG GET bind
# 给某一项配置设置值
redis 127.0.0.1:6379> CONFIG SET loglevel "notice"
```

配置说明：

1. Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程

​    **daemonize no**

2. 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定

​    **pidfile /var/run/redis.pid**

3. 指定Redis监听端口，默认端口为6379，作者在自己的一篇博文中解释了为什么选用6379作为默认端口，因为6379手机按键上MERZ对应的号码，而MERZ取自意大利歌女Alessia Merz的名字

​    **port 6379**

4. 绑定的主机地址

​    **bind 127.0.0.1**

5.当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能

​    **timeout 300**

6. 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose

​    **loglevel verbose**

7. 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null

​    **logfile stdout**

8. 设置数据库的数量，默认数据库为0，可以使用SELECT `<dbid>`命令在连接上指定数据库id

​    **databases 16**

9. 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合

​    **save <seconds> <changes>**

​    Redis默认配置文件中提供了三个条件：

​    **save 900 1**

​    **save 300 10**

​    **save 60 10000**

​    分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改。

10. 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大

​    **rdbcompression yes**

11. RDB文件是否需要CRC64校验, 若为yes,会在生成RDB文件后计算其CRC64并将结果追加至文件尾，同样，Redis启动Load RDB时，也会先计算该文件的CRC64并与dump时的计算结果对比。优点：可以严格保证RDB的完整性及安全性。缺点：会在dump或load时损失10%的性能。如果要最大化Redis的性能，这个配置项应该用no关掉。

    **rdbchecksum yes**

12. 指定本地数据库文件名，默认值为dump.rdb

​    **dbfilename dump.rdb**

13. 指定本地数据库存放目录

​    **dir ./**

14. 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步

​    **slaveof <masterip> <masterport>**

15. 当master服务设置了密码保护时，slav服务连接master的密码

​    **masterauth <master-password>**

16. 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH `<password>`命令提供密码，默认关闭

​    **requirepass foobared**

17. 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息

​    **maxclients 128**

18. 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区

​    **maxmemory <bytes>**

19. 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no

​    **appendonly no**

20. 指定更新日志文件名，默认为appendonly.aof

​     **appendfilename appendonly.aof**

21. 指定更新日志条件，共有3个可选值：     **no**：表示等操作系统进行数据缓存同步到磁盘（快）     **always**：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）     **everysec**：表示每秒同步一次（折衷，默认值）

​    **appendfsync everysec**

22. 指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析Redis的VM机制）

​     **vm-enabled no**

23. 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享

​     **vm-swap-file /tmp/redis.swap**

24. 将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据 就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0

​     **vm-max-memory 0**

25. Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很大大对象，则可以使用更大的page，如果不 确定，就使用默认值

​     **vm-page-size 32**

26. 设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，，在磁盘上每8个pages将消耗1byte的内存。

​     **vm-pages 134217728**

27 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4

​     **vm-max-threads 4**

28. 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启

​    **glueoutputbuf yes**

29. 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法

​    **hash-max-zipmap-entries 64**

​    **hash-max-zipmap-value 512**

30 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）

​    **activerehashing yes**

31. 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件

​    **include /path/to/local.conf**



## redis持久化

redis支持两种持久化策略，分别是RDB和AOF。

### RDB

可以直接使用命令`save`立即生成一个rdb文件。请注意，**这会阻塞服务器进程**，直至rdb文件创建完毕。

RDB方式会根据用户在配置项里的` save <seconds> <changes>`来进行自动生成rdb文件。

### AOF





## redis指令

[指令文档](http://www.redis.net.cn/tutorial/3501.html) [官方文档](https://redis.io/documentation)