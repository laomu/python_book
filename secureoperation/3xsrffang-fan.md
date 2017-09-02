# XSRF跨域请求伪造防范

### 1. 什么是XSRF

### 2. XSRF安全设置

在tornado项目中启用xsrf安全防范，需要在Application服务对象中添加xsrf开关

xsrf设置能在一定程度上，防范异常的post请求进行的数据设置操作。

> 备注：在操作xsrf之前，请确保添加了cookie\_secret混淆秘钥，两者配合才能完成XSRF请求防范

```
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

### 3. XSRF请求操作



