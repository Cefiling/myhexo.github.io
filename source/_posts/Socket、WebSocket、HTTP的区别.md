---
title: Socket、WebSocket、HTTP的区别
date: 2020-11-15 22:01:14
tags:
---

# Socket、WebSocket 、HTTP的区别

1. websocket与socke

   ​		WebSocket是HTML5规范提出的一种协议；目前除了卸载不掉的IE浏览器，其他浏览器都基本支持。他是一种基于TCP协议的，和HTTP协议是并存的两种协议。

   ​		HTML5 Web Sockets规范定义了Web Sockets API，支持页面使用Web Socket协议与远程主机进行全双工的通信。它引入了WebSocket接口并且定义了一个全双工的通信通道，通过一个单一的套接字在Web上进行操作。HTML5 Web Sockets以最小的开销高效地提供了Web连接。相较于经常需要使用推送实时数据到客户端甚至通过维护两个HTTP连接来模拟全双工连接的旧的轮询或长轮询（Comet）来说，这就极大的减少了不必要的网络流量与延迟。

   ​		要使用HTML5 Web Sockets从一个Web客户端连接到一个远程端点，你要创建一个新的WebSocket实例并为之提供一个URL来表示你想要连接到的远程端点。该规范定义了ws://以及wss://模式来分别表示WebSocket和安全WebSocket连接,这就跟http:// 以及https:// 的区别是差不多的。一个WebSocket连接是在客户端与服务器之间HTTP协议的初始握手阶段将其升级到Web Socket协议来建立的，其底层仍是TCP/IP连接。

   ​		首先，socket并不是一个协议，他存在于OSI模型的会话层，是为了方便人们直接使用TCP或UDP而存在的一个抽象层，是位于应用层和传输层之间的一组接口。

   ​		所以，Socket是传输控制层接口，websocket是应用层协议。

2. HTTP与websocket

   ​		HTTP和websocket都是基于TCP协议的可靠性传输协议，他们都是应用层协议。

   ​		不同点：

   ​		websocket是双向通信协议，是长连接，一般不会断开连接。HTTP是单向的，每发一次请求，收到一次响应，连接就结束了。

   ​		websocket需要浏览器和服务器握手建立连接，而HTTP是浏览器发起向服务器的连接，服务器预先不知道这个链接。

3. websocket的应用

   ​		最常见的直播就是利用websocket进行实时连接，websocket为了保持双向通信，需要确保客户端与服务端之间的TCP通道保持连接，缺点就是如果长时间没有来往数据，会浪费连接资源。

   ​		还有消息推送的功能，比如当你每有一个待办消息时，服务器可以实时的发消息通知你，这也是基于websocket

需要注意的一点：websocket发送文字的时候必须要utf-8进行编码。如果是和redis交互，就要了解redis取出来的数据是bytes类型，需要先decode（）转换成字符串，在进行encode（utf-8）编码。