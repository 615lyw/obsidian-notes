---
date created: 2022-07-14, 11:02:51
date modified: 2022-07-14, 11:02:54
---

# Meta

- alias:
- parent :: [[Linux]]
- siblings ::
- child ::
- refs: 
    - [ssh免密登录原理与实现 - 腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1484468#:~:text=%E5%85%8D%E5%AF%86%E7%99%BB%E5%BD%95%EF%BC%88%E7%A7%98%E9%92%A5%EF%BC%89%E9%AA%8C%E8%AF%81&text=%E5%BD%93%E5%AE%A2%E6%88%B7%E7%AB%AFSSH%E8%BF%9E%E6%8E%A5,%E7%84%B6%E5%90%8E%E5%8F%91%E9%80%81%E7%BB%99%E5%AE%A2%E6%88%B7%E7%AB%AF%E3%80%82)
    - [Linux之SSH免密登录配置_恒悦sunsite的博客-CSDN博客_linux免密登录ssh](https://blog.csdn.net/carefree2005/article/details/111679673)

---

# 原理

服务器 A 想要免密登录服务器 B：

1. 服务器 A 加入服务器 B 的免密主机登录列表（即这个列表里的主机都能免密登录）
2. 证明自己是服务器 A

ssh 底层是 SSL，本质是用 [[加解密算法#对称加密与非对称加密]]。

# How

参见 refs。