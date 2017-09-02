# cookie操作

cookie在web应用开发过程中，经常用于会话跟踪技术的使用

如：我们观看网页上视频的进度：当我们关闭了正在观看视频的网页，下次打开网页查看视频时视频从上次停止的地方直接播放了；查看浏览历史：可以在电商网站或者其他网站上，查看自己浏览网页的历史记录等等；记住用户登录状态：某些情况下可以在登录账号时选择记住密码三天、六天......

在tornado.web.RequestHandler中已经封装了对cookie的操作

### 1. 设置cookie : set\_cookie\(\*args\)

**set**_**cookie\(name, value, domain=None, expires=None, path="/", expires\_days=None**_**\)**

使用到的参数说明如下：

| 参数名称 | 描述 |
| :--- | :--- |
| name | cookie名称 |
| value | cookie值 |
| domain | 保存cookie的域名 |
| expires | cookie的过期时间，数值可以是时间戳整数，可以是时间元组，可以是datetime类型，**UTC时间** |
| expires\_days | cookie的过期时间，天数 |
| path | 保存cookie的路径 |

那么，接下来，我们先看如下代码，通过不同的方式保存cookie：

```
# -*- coding:utf-8 -*-

from tornado.web import Application, RequestHandler
from tornado.ioloop import IOLoop
import time


class IndexHandler(RequestHandler):
    def get(self):
        self.set_cookie("c1", "v1")
        self.set_cookie("c2", "v2", domain="localhost")
        self.set_cookie("c3", "v3", path="/damu")
        self.set_cookie("c4", "v4", expires=time.strptime("2017-10-10 23:59:59", "%Y-%m-%d %H:%M:%S"))
        self.set_cookie("c5", "v5", expires_days=10)
        self.set_cookie("c6", "v6", expires=time.mktime(time.strptime("2017-10-10 23:59:59", "%Y-%m-%d %H:%M:%S")))
        self.write("request access successfully!")


if __name__ == "__main__":
    app = Application([(r"/", IndexHandler)])
    app.listen(8888)
    IOLoop.current().start()
```

运行程序，打开浏览器，访问[http://localhost:8888](http://localhost:8888)查看结果：

如下图查看访问网站时存储的cookie数据，查看每一个保存的cookie数据有什么不一样。

![](/assets/cookie01)

