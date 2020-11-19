---
title: 对Cookie、session、JWT的理解
date: 2020-11-01 19:55:24
tags:
---

# 对Cookie、session、JWT的理解

​		相信这三种用户认证的方式，会经常在面试中被问到，以下是我个人对这三种认证方式的一些理解。

​		首先是Cookie，Cookie是保存在用户浏览器端，以JSON的形式保存用户名和密码等明文信息。每次用户登录后，服务器会将用户信息返回给客户端（也就是我们所说的浏览器），客户端会将这些信息以JSON的形式保存下来，之后每次用户访问网页时，都会携带Cookie，当服务器收到Cookie时，会确认Cookie中的信息，来确认用户。

​		session和Cookie类似，每次用户登录后，服务器会生成一条数据，里面含有`session_id`和`session_data`，`session_data`里面保存着用户的信息，但是服务器只会将`session_id`返回给客户端，客户端会将`session_id`保存在Cookie里。之后用户每次访问网页时，都会携带含有`session_id`的Cookie，服务器通过`session_id`来查找对应的`session_data`，从`session_data`里取出数据来确认用户信息。

这就是Cookie和Session的认证过程。当然我们还需要确认两者之间的区别：

```python
1. 存放位置不同
	Cookie保存在客户端，Session保存在服务端。
2. 存取方式不同
	Cookie只能保存ASCII字符串，如果需存储其他类型需要编码，最大能存储4KB
    Session能存储任何类型的数据，理论上没有大小限制
3. 安全性
	Cookie存储在浏览器，对客户端可见，容易泄露信息。
    Session存储在服务器，不存在信息泄露。
4. 对服务器造成的压力不同
	Cookie保存在客户端，不占用服务器资源。
    Session保存在服务器，每个用户都会产生一个Session，如果并发访问的用户很多，会耗费大量内存。
5. 跨域支持上的不同
	Cookie支持跨域名访问。
    Session不支持跨域名访问，Session仅在他所在的域名内有效。
```



最后再说一下JWT吧。JWT类似于一个异步的过程，每次用户登录后，服务器会根据用户信息生成一个JWT的`token`，并返回给客户端。这个`token`是由两个`.`分割成三个部分的字符串，其中的签名里会包含用户经过加密的信息和加密的方式。客户端会将值保存在Cookie里。之后我们每次访问网页时，都会携带含有`token`的Cookie，服务器收到`token`后，通过解密来获得用户信息，从而进行用户认证。

