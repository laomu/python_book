---
author: 牟文斌
qq: 1007821300
email: muwenbin@qikux.com
version: V1.00
desc: 课程教案
---

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

添加了xsrf防范之后，我们不仅仅屏蔽了第三方非法的访问，同时连我们自己的访问也屏蔽了，这不是我们想要的结果，针对tornado的访问操作，我们就页面模板应用和非模板应用两种操作方式进行介绍

##### 3.1. 模板应用

模板应用中，如果常规的表单提交时，由于添加了xsrf\_cookies跨域请求限制，导致请求出现403被拒绝的情况，所以在模板应用中，我们通过添加xsrf验证来解决这个问题

```
{% module xsrf_form_html()%}
```

此时，我们通过模板页面中表单的改造，完成xsrf防范下的表单请求问题：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>模板页面</title>
</head>
<body>
<form action="/" method="post">
    <!-- 模板中添加xsrf验证 -->
    {% module xsrf_form_html() %}
    name: <input type="text" name="name">
    msg: <input type="text" name="msg">
    <input type="submit" value="提交">
</form>
</body>
</html>
```

添加验证之后，通过模板提交请求，数据就能正常访问了，在通过get方式代开渲染的页面时，看到在表单中就自动添加了xsrf隐藏字段作为表单的一部分![](/assets/xsrf2)表单中填入需要提交的数据，点击提交按钮提交表单，数据正常提交并可以正常访问页面![](/assets/xsrf1)

##### 3.2. 非模板应用



