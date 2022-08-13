---
date created: 2021-09-01, 14:38:32
date modified: 2022-08-10, 12:32:42
---

# Meta

- alias:
- parent :: [[计算机网络]]
- siblings ::
- child ::
- refs:
    - [3.1 HTTP 常见面试题 | 小林coding](https://xiaolincoding.com/network/2_http/http_interview.html#http-%E4%B8%8E-https)
    - [3.3 HTTPS RSA 握手解析 | 小林coding](https://xiaolincoding.com/network/2_http/https_rsa.html#tls-%E6%8F%A1%E6%89%8B%E8%BF%87%E7%A8%8B)

---

# HTTPS

解决了 [[MOC HTTP|HTTP]] 面临的安全问题。

在 TCP 和 HTTP 之间加入了 SSL/TLS 安全协议。

HTTPS 建立连接的具体过程：

- 客户端产生随机数 A 发送给服务端
- 服务端产生随机数 B 和数字证书一起发送给客户端
- **客户端验证数字证书** [[加解密算法#证书的签发和验证流程]]，产生随机数 C 发送给服务端
- 双方各自根据随机数 A、B、C 和加密算法产生会话秘钥
- 后续 HTTP 通信使用会话密钥进行对称加密通信
