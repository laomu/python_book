---
author: 牟文斌
qq: 1007821300
email: muwenbin@qikux.com
version: V1.00
desc: 课程教案
---

# 入门程序

准备好了，哪我们就开始吧！

先确定好你的开发目录，进入开发目录后，创建我们的第一个tornado程序文件：server.py

```
/home/py_workspace/tornado/lessons01/server.py
```

创建好我们的文件之后，编辑server.py，添加如下代码

```
# -*- coding:utf-8 -*-

# 引入依赖的模块
import tornado.web
import tornado.ioloop


# 定义处理类
class MainHandler(tornado.web.RequestHandler):
    # 定义处理get请求的方法
    def get(self):
        # 向页面中写入数据
        self.write("hello tornado!")
        
if __name__ == "__main__":
    # 创建web应用对象
    app = Application(
        # 定义一个路由
        [(r"/", MainHandler)]
    )
    # 绑定监听端口
    app.listen(8000)
    # 启动服务
    tornado.ioloop.IOLoop.curren().start()
```



