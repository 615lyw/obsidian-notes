# Meta

- alias: 
- parent :: 
- siblings :: 
- child :: 
- refs: https://ubuntu.com/server/docs/network-configuration

---

修改：`/etc/netplan/xxx_config.yaml`

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      addresses:
        - 10.10.10.2/24
      gateway4: 10.10.10.1
```

执行 `sudo netplan apply`