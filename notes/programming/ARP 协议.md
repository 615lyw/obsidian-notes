---
date created: 2022-06-15, 19:57:28
date modified: 2022-06-15, 20:00:42
---

# Meta

- alias:
- parent :: [[网络层]]
- siblings ::
- child ::
- refs: [5.1 IP 基础知识全家桶 | 小林coding](https://xiaolincoding.com/network/4_ip/ip_base.html#arp)

---

- 用于已知目标 IP 地址获取目标 MAC 地址
- 过程
    - 主机广播 ARP 请求
    - ARP 响应：若目标 IP 地址是自己，则响应自己的 MAC 地址
- MAC 地址会被缓存
