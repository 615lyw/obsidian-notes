# Runnable

JDK 1.0 引入的。

```
@FunctionalInterface
public interface Runnable { 
	// 没有返回值也无法抛出 checkedException
	public abstract void run();
}
```

# Callable

JDK 1.5 随并发包引入的。

```
@FunctionalInterface  
public interface Callable<V> {  
	// 可以有返回值；可以抛出 checkedException
	V call() throws Exception;  
}
```

故需要有返回值或抛出受检异常时使用 Callable 接口。
