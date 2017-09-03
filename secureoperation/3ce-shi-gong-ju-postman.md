---
author: 牟文斌
qq: 1007821300
email: muwenbin@qikux.com
version: V1.00
desc: 课程教案
---

# PostMan请求测试工具

PostMan是一个免费开源的测试工具，在使用的过程中，主要是通过添加成Google的插件应用来操作的

打开谷歌浏览器，在设置-&gt;扩展程序中，打开谷歌应用商店，搜索postman，在应用中找到Postman应用然后添加到应用即可。

> 备注：google浏览器应用商店是被屏蔽的网站，国内访问是需要翻墙才能正常进行操作的，时下比较流行的翻墙软件如蓝灯、XXNET、赛风等等，都可以使用

添加完成后，可以在谷歌浏览器打开应用管理界面，就能看到对应的添加的应用了

![](/assets/post1)

### 1. 界面介绍

点击打开postman应用，第一次打开会要求使用账号登录，这里选择下面提供的选项以后登录

![](/assets/postman2)

### 2. 使用postman发送get请求

接下来，我们先通过postman发送一个get请求，来观察postman的使用方式：选择GET请求方式，在地址栏中输入我们之前开发的get\(\)方式的请求接口地址：[http://localhost:8888，点击send发送请求。](http://localhost:8888，点击send发送请求。)

在Response响应敞口我们可以看到服务器返回的响应数据已经展示出来了![](/assets/postman31)

> 备注：如果在发送get请求的同时需要发送参数，可以点击发送按钮前面的Params，在地址栏下面的输入框中输入参数的名称和值，key对应发送参数的参数名称，value对应发送参数的数据。

### 3. 使用postman发送post请求

post请求方式和get方式大同小异，请求方式进行修改，同时发送的参数的位置稍有不同，其他设置一致![](/assets/postman4)

