序列化是对象持久化的一种手段。使用数据库存储对象的属性也是一种持久化手段。

序列化：将对象转换为特殊格式（可以为文本格式，例如 JSON；或二进制流）。用于持久化至磁盘或网络传输。

反序列化：将特殊格式数据还原为 JVM 能够操作的对象。

---

**为什么需要持久化对象？**

在 Java 程序中，创建完后的对象存活于 JVM 堆内存中，一旦 JVM 停止运行，则对象数据全部丢失。若需要 JVM 停止运行之后能够保存 (持久化) 指定的对象，并在将来重新读取被保存的对象，或是需要把对象通过网络传输给另一个 JVM。

**持久化对象哪些信息？**

通过 Java 对象序列化保存对象时，保存的是对象的状态（即对象的成员变量），会把对象状态转变为一组字节，将来再通过这组字节重新生成对象。

**为什么不需要保存静态变量？**

静态变量是属于类的，在类加载时完成初始化。

**对象转 JSON 也是一种序列化方式，为什么不用实现 `Serializable` 接口？**

实现 `Serializable` 接口的对象是需要转换为字节序列用于网络传输或持久化。

而对象转 JSON，本质是把对象转成一种标准文本型数据格式，网络传输的是文本内容。

# 如何将 Java 对象序列化与反序列化

## 实现 Serializable 接口

1. 只要一个类实现了 Serializable 接口就能被序列化。若未实现，序列化时会抛 `NotSerializableException`；
2. 通过 `ObjectOutputStream` 和 `ObjectInputStream` 对对象进行序列化和反序列化；
3. [[transient]] 阻止变量参与序列化过程；反序列化时 transient 声明的变量会被设定为初始值（[[Java 面向对象#对象初始化过程]]），如 int 型的是 0，对象型的是 null；
4. 可序列化类的所有⼦类也可序列化（子类间接实现了 Serializable 接口）；
5. 如果要序列化的类有父类，要想同时将在父类中定义过的变量持久化下来，那么父类也应该实现 Serializable 接口；

```java
@Data
public class User implements Serializable {
    private String name;
    private int age;
	private transient Boolean isMale;
}

public class SerializableDemo {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        User user = new User();
        user.setName("liuyiwei");
        user.setAge(24);
		user.setIsMale(Boolean.TRUE);
        System.out.println("序列化前：" + user);

        serializeObject(user);

        User deserializedUser = deserializeObject();
        System.out.println("反序列化后：" + deserializedUser);
    }

    private static User deserializeObject() throws IOException, ClassNotFoundException {
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("ObjectFile"));
        return (User) ois.readObject();
    }

    private static void serializeObject(User user) throws IOException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("ObjectFile"));
        oos.writeObject(user);
        oos.close();
    }
}

// 输出：
// 序列化前：User(name=liuyiwei, age=24, isMale=true)
// 反序列化后：User(name=liuyiwei, age=24, isMale=null)
```

## 实现 Externalizable 接口

TODO

# serialVersionUID

序列化版本控制字段，简称 suid：用于验证类的版本一致性，通常用于控制新类和旧类的版本兼容性。

类可以显式指定 serialVersionUID 值，如：

```java
private static final long serialVersionUID = 1L;
```

根据 `getDeclaredSUID()` 可知 serialVersionUID 格式是固定的，必要元素：

- serialVersionUID 变量名
- 被 final 和 static 关键字修饰
- long 类型

```java
public class ObjectStreamClass {
    /**
     * Returns explicit serial version UID value declared by given class, or
     * null if none.
     */
    private static Long getDeclaredSUID(Class<?> cl) {
        try {
            Field f = cl.getDeclaredField("serialVersionUID");
            int mask = Modifier.STATIC | Modifier.FINAL;
            if ((f.getModifiers() & mask) == mask) {
                f.setAccessible(true);
                return Long.valueOf(f.getLong(null));
            }
        } catch (Exception ex) {
        }
        return null;
    }
	
	public long getSerialVersionUID() {
        // REMIND: synchronize instead of relying on volatile?
        if (suid == null) {
            suid = AccessController.doPrivileged(
                new PrivilegedAction<Long>() {
                    public Long run() {
                        return computeDefaultSUID(cl);
                    }
                }
            );
        }
        return suid.longValue();
    }
}
```

当类没有显式指定 serialVersionUID，则通过 `computeDefaultSUID` 方法计算 serialVersionUID 值。计算规则与类名、类的成员变量、类的方法等有关。

## 为什么不要轻易改动类的 serialVersionUID 或要显式指定 serialVersionUID？

若类没有显示指定 serialVersionUID，一旦序列化后改动类代码会造成反序列化时计算的 serialVersionUID 和序列化前计算的不一致，导致抛 `InvalidClassException` 异常。

## 如何使用 IDEA 自动生成 serialVersionUID

[[配置 IDEA 自动生成 serialVersionUID]]
