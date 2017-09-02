# SECRET COOKIE

cookie数据是保存在浏览器客户端所在电脑中的文本文件，很容易造成cookie数据泄露或者被伪造篡改等一系列安全问题。

Tornado提供了一种cookie的安全操作策略，可以通过给cookie添加混淆秘钥cookie\_secret，生成被加密操作之后的cookie



