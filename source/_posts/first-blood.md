---
title: 如何搭建一个自己的博客？
---
一直都想拥有自己的博客，就想着记录一些生活小事，像是再写日记一样，等到时间久了，发现了那么多的文字和日期，一定感觉很好。

今天终于实现啦，而且这个博客定位属于技术博客，专注于分享技术问题的解决经验和新技术的学习总结。

---

导读

- [如何搭建一个自己的博客？](#a)
- [如何优雅的管理图片？](#b)
- [如何开启博客rss订阅功能？](#c)
- [如何使用systemctl管理服务器开机启动项？](#d)

---

### <span id='a'>如何搭建一个自己的博客？</span>

1.购买服务器

说一下自己的情况，我购买的是阿里云服务器ECS，下面是一些配置信息，花了200多包年，因为是学生所以很划算~

<img src="1.png" width = "300" alt="基本信息"/>
<img src="2.png" width = "300" alt="配置信息"/>

2.初始化本地blog仓库

我使用[hexo](https://github.com/hexojs/hexo)，来生成和管理博客站点代码

``` bash
$ yarn add hexo-cli
```

``` bash
$ ./node_modules/.bin/hexo init blog && cd blog
```
package.json的配置
```javascript
 "scripts": {
    "server": "./node_modules/.bin/hexo server",
    "new": "./node_modules/.bin/hexo new",
    "generate": "./node_modules/.bin/hexo generate",
    "deploy": "rsync -vzrtopgl --progress ./public/* root@106.14.174.157:/root/blog"
}
```

---

### <span id='b'>如何优雅的管理图片？</span>

1.[开启_config.yml的post_asset_folder](https://hexo.io/zh-cn/docs/asset-folders.html)

2.安装[hexo-asset-image](https://github.com/CodeFalling/hexo-asset-image)

``` bash
$ yarn add https://github.com/CodeFalling/hexo-asset-image
```

3.使用语法如下

<img src="3.png" width = "200" alt="目录结构"/>
<img src="4.png" width = "500" alt="引入方式"/>

---

### <span id='c'>如何开启博客rss订阅功能？</span>

1.安装[hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)

``` bash
$ yarn add hexo-generator-feed
```

2.配置`_config.yml`

``` bash
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
```

---

### <span id='d'>如何使用[systemctl](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)管理服务器开机启动项？</span>

ps:感谢李宇飞巨佬的友情帮助

1.配置服务文件

``` bash
$ vim /usr/lib/systemd/system/blog.service
```

``` bash
[Unit]
Description=你项目的描述信息

[Service]
User=root
WorkingDirectory=命令执行的目录
ExecStart=启动命令
Environment="环境变量=xxx"
KillMode=process
Restart=on-failure
RestartSec=16s

[Install]
WantedBy=multi-user.target
```
> 注意那句 /bin/bash -c 表示以 bash 启动，它会先加载用户服务器的环境 (/etc/profile、/root/.bash_profile、/root/.bashrc)，比如 npm 路径之类的，可以防止找不到 npm 命令

2.使用systemctl命令

> systemctl 是个系统命令，可以管理服务器上的任意服务

重载配置文件
``` bash
$ systemctl daemon-reload
```

设置开机自启动
``` bash
$ systemctl enable blog
```

启动
``` bash
$ systemctl start blog
```

可以查看服务状态
``` bash
$ systemctl status blog
```

可以查看服务日志
``` bash
$ journalctl -u blog -xfl 
```

查看进程
``` bash
$ ps -ef
```

