---
date created: 2022-06-05, 22:45:47
date modified: 2022-06-05, 22:56:17
---

# Meta

- parent :: [[计算机网络]]
- siblings ::
- child ::
- refs: [2.2 键入网址到网页显示，期间发生了什么？ | 小林coding](https://xiaolincoding.com/network/1_base/what_happen_url.html#%E7%9C%9F%E5%AE%9E%E5%9C%B0%E5%9D%80%E6%9F%A5%E8%AF%A2-dns)

---

先找缓存，再找 DNS 服务器。

缓存查找顺序：

1. 浏览器
2. 操作系统
3. hosts 文件

查找 DNS 服务器过程：

1. 浏览器发起 DNS 请求，查找本地 DNS 服务器（客户端自己在网络模块设置）
2. 找根域名服务器获得顶级域名服务器地址
3. 找顶级域名服务器获得权威 DNS 服务器地址
4. 权威 DNS 服务器把 IP 返给本地 DNS 服务器
5. 本地 DNS 服务器把 IP 地址返给客户端
