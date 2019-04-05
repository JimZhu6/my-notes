

# tg-bot小记

## 生成bot

私聊[@BotFather](https://t.me/BotFather)，输入`/newbot`，按顺序输入bot的名称和用户名，就会返回机器人的token。



## 请求机器人api

[机器人api文档](https://core.telegram.org/bots/api)

所有的请求api格式都是`https://api.telegram.org/bot<Token>/<Method Name>`的格式。

如：`https://api.telegram.org/bot123abc/getMe`

所有请求都支持`https`的`post`与`get`。请求类型可选四种：

- 字符串拼接的方式：

  如：`https://api.telegram.org/bot123abc/sendMessage?chat_id=123&text=hello`

- application/json

  通过json格式发送请求，可读性较强

- application/x-www-form-urlencoded

- multipart/form-data

  上传文件只能选择这种方式。

返回值是以json格式数据，包含一个`ok`以及`result`，如果请求错误，则会通过`descript`或`error_code`或`retry_after`返回错误信息。



## 使用node.js编写bot

使用npm包`npm install --save node-telegram-bot-api`

文档地址：https://github.com/yagop/node-telegram-bot-api

注意需要填写正确的机器人`token`



## 使用heroku构建测试环境

[heroku官网](https://www.heroku.com/)，先去注册个账号

项目流程：

- 创建本地项目文件夹

- 初始化npm包管理工具

- 在`package.json`文件新建一个事件`"start":"node index.js"`指向项目入口js文件

- 在入口js文件添加一个监听端口事件，目的是让heroku能存活时间长一点（付费用户应该可以无视）

  ```js
  const express = require("express");
  const app = express();
  const port = process.env.PORT;
  app.listen(port);
  ```

- 初始化git仓库并进行一次提交

- 建立heroku app ：`heroku create`

- 执行部署命令：`git push heroku master`

在部署后，如果想查看实时日志，可执行命令`heroku logs --tail`获取

更多信息参考[heroku官方文档](https://devcenter.heroku.com/articles/getting-started-with-nodejs)

