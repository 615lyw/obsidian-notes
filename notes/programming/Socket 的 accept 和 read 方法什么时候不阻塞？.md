---
date created: 2022-05-18, 21:14:59
date modified: 2022-05-22, 09:55:14
---

# Meta

- parent:: [[Socket]]
- siblings::
- child::
- refs:

---

# Socket 的 accept 和 read 方法什么时候不阻塞？

主要关注下面代码 accept 方法和 read 方法的阻塞等待部分，accept 只要客户端发起连接就不再阻塞，read 只要客户端发送了数据便不阻塞。

```java
public class Server {
  public static void main(String[] args) {
      try {
          ServerSocket ss = new ServerSocket(8888);
          System.out.println("监听端口号: 8888");
          // 阻塞等待客户端请求
          // Client 执行 Socket s = new Socket("127.0.0.1", 8888); 此处便会接受到请求
          Socket s = ss.accept();
          InputStream is = s.getInputStream();
          // 阻塞等待客户端发来的数据
          // Client 执行 os.write(110); 此处便不再阻塞
          int msg = is.read();
          System.out.println(msg);
          is.close();
          s.close();
          ss.close();
      } catch (IOException e) {
          e.printStackTrace();
      }
  }
}

public class Client {	  
    public static void main(String[] args) {
      try {
          Socket s = new Socket("127.0.0.1", 8888);
          // 打开输出流
          OutputStream os = s.getOutputStream();
          // 发送数字110到服务端
          os.write(110);
          os.close();
          s.close();
      } catch (IOException e) {
          e.printStackTrace();
      }
    }
}
```
