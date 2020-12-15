# Cocos2d-js

> [文档](https://docs.cocos.com/cocos2d-x/manual/zh/) [Github](https://github.com/cocos2d/cocos2d-x) [API](https://docs.cocos2d-x.org/api-ref/index.html) [论坛](https://forum.cocos.org/c/cocos2d-x/16)

## 安装控制台

[引擎包下载地址](https://www.cocos.com/cocos2dx) [JDK下载地址](https://www.oracle.com/cn/java/technologies/javase-downloads.html) [Apache Ant](https://ant.apache.org/bindownload.cgi)（如需导入Ant，需要定位到里面的bin文件夹） [Android NDK](https://developer.android.com/ndk/downloads) [SDK](https://www.androiddevtools.cn/)

下载引擎包，执行引擎包内的`setup.py`按提示导入Ant、NDK、SDK。



## 常用指令

[docs](https://docs.cocos.com/cocos2d-x/manual/zh/editors_and_tools/cocosCLTool.html)

### 项目创建

```sh
cocos new <game name> -p <package identifier> -l <language> -d <location>
```

### 项目编译

```sh
cocos compile -s <path to your project> -p <platform> -m <mode> -o <output directory>
```

### 项目运行

```sh
cocos run -s <path to your project> -p <platform>
```

### 项目发布

```sh
cocos deploy -s <path to your project> -p <platform> -m <mode>
```

### 更多帮助

运行 `cocos new --help`、`cocos compile --help`、`cocos deploy --help`等...可以查看更多帮助信息。