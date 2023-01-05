# RPC 原理

RPC（Remote Procedure Call） 即远程过程调用。

**目的在于让远程方法调用像本地方法调用一样简单。**

![[Pasted image 20220408175306.png]]

核心即 client stub 和 server stub，作为 client 和底层网络通信的中间层，封装底层网络传输和方法传参、解析结果等细节。

## 实现关键点

[[RPC 实现关键点]]

## 参考

[[CleanShot 2022-04-09 at 07.24.15@2x.png]]