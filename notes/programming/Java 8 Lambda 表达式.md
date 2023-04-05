---
date created: 2021-11-21, 15:47:59
date modified: 2022-06-01, 10:14:58
---

# Meta

- parent :: [[Java 8 新特性]]
- siblings ::
- child ::
- refs:

---

# Lambda 使用场景

主要是在使用类库函数，形参为函数式接口时。传统写法需要定义一个匿名内部类并重写其中的方法，如今自定义一个 Lambda 表达式或者通过方法引用作为实参传进去。

# Lambda 表达式两种形式

## 写法一

适用于没有现成的方法时：

```java
(类型 变量名, ....) -> {
	// 函数体
}
```

例子：`BinaryOperator<Long> addExplicit = (Long x, Long y) -> x + y;`

创建了一个匿名函数，用于两变量相加，赋值给了一个 `BinaryOperator<Long>` 类型的变量。

## 写法二：方法引用

如果有现成的方法可以作为 Lambda 的实参时，可以通过方法引用传入。

- `Class::staticMethod`
- `instance::normalMethod`

# Lambda 本质

**只有一个方法的对象实例。（本质为对象在 Java 中是一等公民，而函数不是；不像 Go，函数也是一等公民）**

```java
public void foo(Predicate<Bar> p) {
	// do some thing
}

// 传统的匿名内部类写法，重写其抽象方法
foo(new Predicate<>() {
	@Override
	public boolean test(Bar b) {
		// 重写 test 方法
	}
})
	
// Lambda 写法：
foo((Bar b) -> {doSomeThing})
```

参数列表为什么不用写类型？因为编译器可以推断。

# Lambda 中可以引用外部变量吗？

可以，但外部变量必须被 `final` 修饰或是事实变量（只被赋值过一次，没有再被修改过），即 Lambda 表达式只能引用值（而非变量）。
