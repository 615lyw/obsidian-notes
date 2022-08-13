---
date created: 2022-05-18, 21:14:59
date modified: 2022-05-22, 09:58:05
---

# Meta

- parent:: [[Socket]]
- siblings::
- child::
- refs:

---

```java
public static byte[] readBytes(InputStream is) throws IOException {
        int bufferSize = 1024;
        byte[] buffer = new byte[bufferSize];
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        
        // 核心部分
        int length;
        while ((length = is.read(buffer)) != -1) {
          baos.write(buffer, 0, length);
        }
        
        return baos.toByteArray();
}
```

上面为读取文件流的通用写法，生效的前提在于文件的大小有上限，最终一定会读取到 `length = -1`，终止循环。

但在 HTTP 双向通信场景中，这种写法不行。因为 http/1.1 默认为持久连接，接收方无法得知发送方本次（一次连接可以发送多次数据）数据是否发送完毕，除非发送方主动关闭输出流或直接关闭 socket 连接。

写法一：发送方在写完数据后调用：`clientSocket.shutdownOutput()`。缺点在于关闭输出后便不能再次写入数据了。

写法二：（前提是发送方发送的数据在被接收方读取时恰好一直满足 1024，一旦小于 1024 即认为发送完毕）

```java
public static byte[] readBytes(InputStream is) throws IOException {
        int bufferSize = 1024;
        byte[] buffer = new byte[bufferSize];
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        
        while (true) {
          int length = is.read(buffer);
          if (-1 == length)
              break;
          baos.write(buffer, 0, length);
          if (length < bufferSize)
              break;
        }
        return baos.toByteArray();
}
```

上面两个方法的局限性：

- 发送方一次性发送完所要发送的数据，不能实现分块传输
- 不能因为网络波动，导致某次缓冲区接收到的数据小于 1024

应该通过请求头中的 `Content-Length` 或 `Transfer-Encoding: chunked` 协议规定来断定一次请求或响应的结束。（参考：[[通过 Content-Length 或 chunked 协议决定一次 HTTP 请求或响应报文结束]] ）