# XSRF跨域请求伪造防范

### 1. 什么是XSRF

### 2. XSRF安全设置

在tornado项目中启用xsrf安全防范，需要在Application服务对象中添加xsrf开关

xsrf设置能在一定程度上，防范异常的post请求进行的数据设置操作。

> 备注：在操作xsrf之前，请确保添加了cookie\_secret混淆秘钥，两者配合才能完成XSRF请求防范

```python
# -*- coding:utf-8 -*-

from tornado.web import Application, RequestHandler
from tornado.ioloop import IOLoop
import uuid, base64


class IndexHandler(RequestHandler):

    def get(self):
        self.set_secure_cookie("sc1", "sv1")
        self.write("ok!")

    def post(self):
        self.write("post ok!")


if __name__ == "__main__":
    cookie_secret = base64.b64encode(uuid.uuid4().bytes + uuid.uuid4().bytes)
    print (cookie_secret)
    app = Application(
        [(r"/", IndexHandler)],
        cookie_secret=cookie_secret,
        xsrf_cookies=True
    )
    app.listen(8888)

    IOLoop.current().start()
```

在Appication中设置了cookie\_secret和xsrf配置之后，我们通过postman测试工具模拟get方式和post方式发送请求，查看设置之后的操作结果

| get请求方式 | post请求方式 |
| :--- | :--- |
| ![](/assets/get) | ![](/assets/post) |

get方式请求数据正常，但是post方式出现了403禁止访问的错误，同时控制台已经出现警告提示信息

```
WARNING:tornado.general:403 POST / (::1): '_xsrf' argument missing from POST
WARNING:tornado.access:403 POST / (::1) 0.49ms
```

### 3. XSRF请求操作



