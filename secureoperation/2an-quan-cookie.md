# SECRET COOKIE

cookie数据是保存在浏览器客户端所在电脑中的文本文件，很容易造成cookie数据泄露或者被伪造篡改等一系列安全问题。

Tornado提供了一种cookie的安全操作策略，可以通过给cookie添加混淆秘钥cookie\_secret，生成被加密操作之后的cookie

要操作安全cookie首先我们得得到一个混淆秘钥，混淆秘钥可以通过UUID来获取，同时使用base64进行编码：

```
>>> import base64, uuid
>>> base64.b64encode(uuid.uuid().bytes + uuid.uuid().bytes)
'pfC0QLEiS/6lsdMeHJf7i9arp6QQLkU1shdMwkvZeVs='
```



