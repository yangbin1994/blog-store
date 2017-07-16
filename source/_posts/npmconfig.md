---
title: npm包管理工具如何配置环境变量？
date: 2017-07-16 16:16:56
tags:
---

今天被环境变量弄的爆炸，原因是对查找规则不清不楚，这里做一个总结。

---

### 如何获得npm的全局安装路径？

```bash
$ npm config get prefix
```

同理如果要设置的话，请看以下

```bash
$ npm config set prefix
```

### 如何在全局执行包命令？

`npm`,`yarn`等包管理工具，在使用`全局安装命令`的时候，会安装到上述提到的目录下，同时会对附带`bin`文件的包，进行注册，将在一个地方提取并保存`bin`文件，我们需要做的就是获得路径并在全局环境变量里配置一下。

获得全局`bin`的路径

```bash
$ npm -g bin / yarn global bin
```

设置到环境变量中

mac os 环境变量加载顺序是

- /etc/profile
- /etc/paths 
- ~/.bash_profile 
- ~/.bash_login 
- ~/.profile 
- ~/.bashrc

这里推荐`/etc/paths`，一行一个简单明了

最后让刚刚修改的配置生效

```bash
$ source /etc/paths
```