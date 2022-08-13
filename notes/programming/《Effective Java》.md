# 异常

## 69. 只针对异常的情况才使用异常

一方面：**异常应用只用于异常的情况下，不要用于正常的控制流。**

另一方面，**在设计 API 时，不要强迫客户端为了正常的控制流而使用异常**。

```java
// 错误写法
try {
	int i = 0;
	while (true) {
		range[i++].climb();
	}
} catch (ArrayIndexOutOfBoundsException e) {

}

// 正确写法
for (Mountain m : range) {
	m.climb();
}

// 设计良好的 API
for (Iterator<Foo> i = collection.iterator(); i.hasNext(); ) {
	Foo foo = i.next();
	...
}

// 设计不好的 API：假如 Iterator 没有提供 hasNext() 状态测试方法
// 客户端不知道什么时候 next() 会抛异常，只好用 try catch 来实现循环正常结束
try {
	Iterator<Foo> i = collection.iterator();
	while (true) {
		Foo foo = i.next();
		...
	}
} catch (NoSuchElementException e) {
	
}
```

如果类具有“状态相关”方法，如 Iterator 的 next()，调用该方法有可能会抛异常且不可预知，客户端只能通过 try catch 来控制程序流，故应该再提供一个“状态测试”方法，用于指示“状态相关”方法能否被调用。（法一）

法二：提供可识别的返回值替代“状态测试”方法。

选择法一还是法二的指导原则：[[TODO]]

## 70.对可恢复的情况使用受检异常，对编程错误使用运行时异常

抛出受检异常编译器会强制 API 使用者要么继续抛出，要么使用 try catch 处理异常。

如果 API 使用者觉得自己没有能力处理该异常则继续向上抛出；
如果 API 使用者使用 try catch 处理异常，则选择让程序从错误中恢复继续运行（而不是终止），此时可以选择自己重试或者是返回错误信息。
