---
title: 跨域请求前如何不发送预检请求OPTIONS？
date: 2017-07-15 12:10:35
tags:
    - CORS
---
今天周六，昨天在公司重构监测代码的时候，遇到一个坑。

给后端上报数据的时候，请求前会发起预检请求OPTIONS。

主要是我很奇怪：网上搜到结果是只要是跨域请求，浏览器就会去做这个处理，调和客户端和服务端的权限关系。

但是目前服务器接口允许的请求方法只有POST，老的监测代码是如何避免OPTIONS请求的呢？

---

### OPTIONS请求的作用是什么？
在跨域请求时，浏览器会事先发送一个 CORS `预检请求`，拿到后端的响应头和客户端的请求头，然后匹配对应规则，做出协调处理。

> 当一个资源从与该资源本身所在的服务器不同的域或端口不同的域或不同的端口请求一个资源时，资源会发起一个跨域 HTTP 请求

### 所有跨域请求浏览器都会拦截吗？
某些请求不会触发 CORS `预检请求`。这样的请求为`简单请求`，请注意，该术语并不属于 Fetch （其中定义了 CORS）规范。若请求满足所有下述条件，则该请求可视为`简单请求`

1. 使用下列方法之一

    - GET
    - HEAD
    - POST
        - 仅当POST方法的Content-Type值等于下列之一才算作简单请求
            - text/plain
            - multipart/form-data
            - application/x-www-form-urlencoded

2. Fetch 规范定义了对 CORS 安全的首部字段集合，不得人为设置该集合之外的其他首部字段。该集合为

    - Accept
    - Accept-Language
    - Content-Language
    - Content-Type （需要注意额外的限制）
    - DPR
    - Downlink
    - Save-Data
    - Viewport-Width
    - Width

参见

- [MDN-HTTP访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)