Meta:

- parents: [[Java 网络编程]]
- status: #New
- refs:
- create time: 2022-05-18 22:19:24
- update time:  2022-05-18 22:19:24

---

Socket 编程是网络应用间通信的基础，分为两个方向：客户端和服务端，客户端（或浏览器）可主动建立 Socket，服务端监听端口等待客户端的 Socket 连接。

浏览器和服务器通过 Socket 进行 HTTP 通信时，以 HTTP 文本信息进行交互，即通过 Socket 获取的输入流是 http 请求，写入输出流时 http 响应。
