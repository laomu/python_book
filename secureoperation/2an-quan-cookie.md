---
author: 牟文斌
qq: 1007821300
email: muwenbin@qikux.com
version: V1.00
desc: 课程教案
---

# SECRET COOKIE

cookie数据是保存在浏览器客户端所在电脑中的文本文件，很容易造成cookie数据泄露或者被伪造篡改等一系列安全问题。 

Tornado提供了一种cookie的安全操作策略，可以通过给cookie添加混淆秘钥cookie\_secret，生成被加密操作之后的cookie

> 切记：Tornado提供的这种通过混淆秘钥进行加密的安全cookie操作，只是在一定程度上提高了安全性，增加了被恶意篡改的难度，但是仅仅这仅仅是签名而不是加密，所以还是有可能存在被抓包窃听，然后伪造cookie进行恶意操作的可能。so，我们在cookie操作时在cookie中还是要避免保存敏感数据的哦！

### 1. 混淆秘钥的设定

要操作安全cookie首先我们得得到一个混淆秘钥，混淆秘钥可以通过UUID来获取，同时使用base64进行编码：

```
>>> import base64, uuid
>>> base64.b64encode(uuid.uuid().bytes + uuid.uuid().bytes)
'pfC0QLEiS/6lsdMeHJf7i9arp6QQLkU1shdMwkvZeVs='
```

有了混淆秘钥之后，我们将该秘钥通过Appliacation的初始化函数中添加配置即可

```python
app = Application(
    [(r"/", Indexhandler)],
    cookie_secret:True
)
```

### 2. 安全cookie操作

```python
set_secret_cookie(name, value, path="/", domain=None, expires_days=1)
通过混淆秘钥设置一个带签名和时间戳的cookie数据，防止cookie伪造
```

```
get_secret_cookie(name, value=None, max_age_days=1)
根据name获取一个安全cookie值，如果没有获取到则返回一个value设定的值
max_age_days可以通过时间过滤符合安全条件的cookie数据
```

可以通过配置cookie\_secret之后，通过cookie安全操作方法进行cookie的赋值和取值

```python
# -*- coding:utf-8 -*-

from tornado.web import Application, RequestHandler
from tornado.ioloop import IOLoop
import uuid, base64


class IndexHandler(RequestHandler):

    def get(self):
        self.set_secure_cookie("sc1", "sv1")
        self.write("ok!")


if __name__ == "__main__":
    cookie_secret = base64.b64encode(uuid.uuid4().bytes + uuid.uuid4().bytes)

    app = Application(
        [(r"/", IndexHandler)],
        cookie_secret=cookie_secret
    )
    app.listen(8888)

    IOLoop.current().start()
```

此时，打开浏览器访问[http://localhost:8888](http://localhost:8888)查看设置的安全cookie数据如下：

![](/assets/secretcookie1)

可以看到，设置了混淆秘钥的cookie值，已经变成了经过秘钥编码操作以后的cookie数据了

```
"2|1:0|10:1504371453|3:sc1|4:c3Yx|be9f9f494971905f4be2a4157f205816653b5171e4e912ca66ab67acd61a4bfa"
```

上述安全cookie值主要由以下几部分组成：

1. 安全cookie版本，默认版本2
2. 默认为0
3. 时间戳
4. cookie名称
5. base64编码的cookie值
6. 签名值



