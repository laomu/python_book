---
author: 牟文斌
qq: 1007821300
email: muwenbin@qikux.com
version: V1.00
desc: 课程教案
---

# 安装tornado

### 1. windows安装

确保你的PC上已经安装好了python环境，并且是联网的状态

那么接下来，打开命令行，执行如下命令

```
> pip install tornado
```

等待下载安装结束， Congratulations，你安装完成了，就是这么简单！

### 2. ubuntu安装

ubuntu系统中，已经内置了python开发环境\(2.x or 3.x\)，默认情况下，是通过软连接链接使用的python2的开发环境，当然，这个不是我们要关注的，我们关注的是你已经在ubuntu上安装好了pip包管理工具

什么？你没有安装pip？你是怎么进行python开发的？没事，现在安装还来得及，打开终端，执行如下命令安装pip（python install package）

```
$ sudo apt-get install python-pip
```

这次应该安装好了，没有什么问题了吧！

那么，在终端继续执行如下命令进行tornado的安装吧

```
$ sudo pip install tornado
```

漫漫长夜无心睡眠，等待联网下载并且自动配置安装结束，恭喜恭喜，ubuntu下，你的tornado也可以使用了。

### 3. mac os安装

如果你使用的是mac系统，基本上应该也是非常有眼光的开发人员了，同时对自己的开发标准定为还是很高的，基本上这一部分就自己解决了！PS：上述几句话是废话

干货请看这里：Mac上已经内置了python的开发环境，但是python官方同样也提示了不要使用mac内置的python开发环境了，而是自己从python官网下载安装一个吧，并且和系统内置的python开发环境不会有什么冲突的！安装过程中可以选择安装pip，或者安装结束后通过easy\_install安装pip，或者通过brew安装pip都是可以的哦。

最后，直接通过如下命令在终端进行tornado安装即可：

```
$ sudo install tornado
```



