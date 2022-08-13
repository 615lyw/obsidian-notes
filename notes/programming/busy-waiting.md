# busy-waiting

在循环中查询是否满足条件。

同义词：【忙等，busy-waiting，循环等待】

例子：

```java
// 轮询查询是否满足条件
while(!testConditon()) {
    ;
}
// 条件满足
doSomething();
```

缺点：如果 while 条件只需执行几次或几十次且每次耗时短即可满足条件退出循环还好。但如果耗时长且执行次数多，则浪费 CPU 资源。

busy-waiting 解决方案：

- [[观察者模式]]
- [[等待-通知机制]]
- [[回调 callback]]
- [[事件 event]]
