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

# Lambda 应用场景

涉及函数式接口的地方。

# Lambda 表现形式

```java
(类型 变量名, ....) -> {
	// 函数体
}
```

例子：`BinaryOperator<Long> addExplicit = (Long x, Long y) -> x + y;`

创建了一个匿名函数，用于两变量相加，赋值给了一个 `BinaryOperator<Long>` 类型的变量。

# Lambda 本质

函数式接口的抽象方法实现。

需要函数式接口的地方有两种写法：

1. 传统匿名内部类写法
2. Lambda 表达式

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

函数式接口的匿名内部类最核心的地方在于唯一的抽象方法实现，方法的核心在于入参和方法的处理逻辑。

如何简单表示一个实现方法？`(参数列表) -> {方法实现}`

参数列表为什么不用写类型？因为编译器可以推断。

# Lambda 中可以引用外部变量吗？

可以，但外部变量必须被 `final` 修饰或是已成事实的变量（只被赋值过一次，没有再被修改过），即 Lambda 表达式只能引用值（而非变量）。
