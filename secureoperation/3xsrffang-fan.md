# XSRF跨域请求伪造防范

### 1. 什么是XSRF

### 2. XSRF安全设置

在tornado项目中启用xsrf安全防范，需要在Application服务对象中添加xsrf开关

xsrf设置能在一定程度上，防范异常的post请求进行的数据设置操作。

> 备注：在操作xsrf之前，请确保添加了cookie\_secret混淆秘钥，两者配合才能完成XSRF请求防范

```

```

### 3. XSRF请求操作



