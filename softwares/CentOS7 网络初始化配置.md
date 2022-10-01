---
date created: 2022-04-27, 23:36:24
date modified: 2022-07-14, 08:24:31
---

# Meta

- alias:
- parent :: [[Linux]]
- siblings ::
- child ::
- refs: [Linux操作系统学习实验环境安装配置视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1bA411b7vs?spm_id_from=333.999.0.0)

---

# 配置虚拟机网络适配器

![CleanShot2022-07-14at07.39.04@2x](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/CleanShot%202022-07-14%20at%2007.39.04@2x.png)

# 获取 IP 地址

执行 `dhclient` 获取一个 IP 地址，通过 `ifconfig` 查看刚获取到的 IP 地址。

# 配置网卡静态 IP

在 `/etc/sysconfig/network-scripts/ifcfg-网卡名称` 文件中配置：

```
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.31.90
NETMASK=255.255.255.0
GATEWAY=192.168.31.1
DNS1=119.29.29.29 // 注意写 DNS 无效，要写 DNS1
```

# 重启网卡

`systemctl restart network.service`
