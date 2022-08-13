---
date created: 2022-05-18, 21:14:59
date modified: 2022-05-23, 14:52:38
---

# Meta

- parent:: [[通过 Class 读取资源文件]]
- siblings::
- child::
- refs: Java 源码

---

# 通过 ClassLoader 或 Class 读取资源文件的区别

`Class.getResourceAsStream()` 对 `ClassLoader.getResourceAsStream()` 进行了封装，增加了对资源文件名的处理。

```java
// Class 源码
public InputStream getResourceAsStream(String name) {
    name = resolveName(name);
	ClassLoader cl = getClassLoader0();
	if (cl == null) {
	    // bootstrap classLoader 没有对应的 classLoader 实例，故 rt.jar 包下的类，例如 String，getClassLoader0() 返回 null
		return ClassLoader.getSystemResourceAsStream(name);
	}
	return cl.getResourceAsStream(name);
}

// 如果文件以 / 开头，则仅去掉 /
// 如果文件不以 / 开头，则添加包名作为文件名前缀路径
private String resolveName(String name) {
    if (name == null) {
        return name;
    }
    if (!name.startsWith("/")) {
        Class<?> c = this;
        while (c.isArray()) {
            c = c.getComponentType();
        }
        String baseName = c.getName();
        int index = baseName.lastIndexOf('.');
        if (index != -1) {
            name = baseName.substring(0, index).replace('.', '/')
                +"/"+name;
        }
    } else {
        name = name.substring(1);
    }
    return name;
}
```

添加包名和不添加包名对比：
`Class.getResource()` 和 `Class.getResourceAsStream()` 内部实现相同，只是返回的类型不同。

![](https://pic-bed-615.oss-cn-beijing.aliyuncs.com/wEpgGi.png)
