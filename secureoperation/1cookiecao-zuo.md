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

```python
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

![](/assets/cookie1)

### 2. 获取cookie : get\_cookie\(\*args\)

**get\_cookie\(name, default=None\)**

获取名为name的cookie，可以设置default默认值，当获取不到name对应的cookie值时，返回default默认值

```python
# -*- coding:utf-8 -*-

from tornado.web import Application, RequestHandler
from tornado.ioloop import IOLoop
import time


class IndexHandler(RequestHandler):
    def get(self):
        ......
        self.write("request access successfully!")


class CookieHandler(RequestHandler):

    def get(self):
        v1 = self.get_cookie("c1")
        v2 = self.get_cookie("c2")
        v3 = self.get_cookie("c3")
        v4 = self.get_cookie("c4")
        v5 = self.get_cookie("c5")
        v6 = self.get_cookie("c6")
        print ("v1: %s" % v1)
        print ("v2: %s" % v2)
        print ("v3: %s" % v3)
        print ("v4: %s" % v4)
        print ("v5: %s" % v5)
        print ("v6: %s" % v6)
        self.write("cookie operation successfully!")

if __name__ == "__main__":
    app = Application([(r"/", IndexHandler), (r"/cookie", CookieHandler)])
    app.listen(8888)
    IOLoop.current().start()
```

执行上述代码，打开浏览器输入访问地址：[http://localhost:8888](http://localhost:8888)给cookie设置值

然后再次访问[http://localhost:8888/cookie访问数据，在控制台得到如下结果：](http://localhost:8888/cookie访问数据，在控制台得到如下结果：)

![](/assets/cookie2)

### 3. 删除cookie ：clear\_cookie\(\*args\)

cookie中可以保存数据，同样也可以删除cookie中的数据

删除数在tornado中有两种操作，删除某一个指定的cookie或者清除所有的cookie

删除cookie的原理：清空cookie中name对应的值，并设置cookie的过期时间让cookie过期，由浏览器删除对应的cookie

```
clear_cookie(name, path="/", domain=None)
删除名称为name，同时匹配path和domain的cookie数据
```

```
clear_all_cookie(name, path="/", domain=None)
删除同时匹配path和domain的所有的cookie值
```



