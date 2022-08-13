# Java 8 新特性

- Lambda
- 流
- 默认方法
- 新的时间和日期 API
- 并发

## 流

- `cat file2 file1 | tr "[A-Z]" "[a-z]" | sort | tail -3`
  - 优点：
    - 在更高层次上写 Java 代码(像写 SQL 一样)，把这样的流转换成那样的流，而不用考虑怎么实现转换
    - 在多核处理器上可以自动实现并行
- 内部迭代与外部迭代
  - 内部迭代：在库里自动迭代
  - 外部迭代：自己写 for-each 循环
- 经典的 Java 程序只能利用一个核

## 行为参数化

- what：可以将函数（方法）作为参数传递给另一方法
- why
  - 让方法更灵活：被传参方法的行为受函数参数的影响（联系”策略模式“）
  - 为接口声明许多只用一次的实体类而造成的啰嗦代码，在 Java 8 之前可以用匿名类来减少
- how：Lambda 表达式

以前对函数的理解为：`f(x)` 对传入的值进行固定流程的处理，函数内部逻辑不变，变化的只是传入的值

行为参数化：一个方法接受多个不同的函数作为参数，并在内部使用它们，完成不同行为的能力。

- 方法引用 `File::isHidden` 语法，即把这个方法作为值

## 匿名函数 Lambda

目的：不想为了一个简短、只在此处用到的方法写很多冗余的类定义以及创建对象。同匿名内部类目的，只不过Lambda 让代码更简洁。

## 默认方法

接口中可以存在默认方法，不强制要求实现类重写。

# Lambda 表达式

函数式接口：只定义了一个抽象方法的接口（不考虑默认方法的存在）。

本质：
- 匿名函数
- **函数式接口的一个实例**
- 抽象方法的具体实现

## what（语法）

`(参数列表) -> 方法主体`

`(参数列表) -> {方法主体;}`

当使用 `{}` 时，里面的方法主体就像写正常函数一样：

- 语句后有分号
- 非 `void` 返回类型必须显式写出 `return`

当不使用 `{}` 时，如果返回的是值类型，`return` 是隐式的。

参数列表和方法主体均可为空。

## when

- 需要用“行为参数化”构建代码
- 函数式接口处

## how

根据函数式接口的目标类型来书写 Lambda 表达式。

**要求：Lambda 表达式的形参列表和方法体的返回值必须与函数式接口中的抽象方法一致！**

## 函数式接口

Java8 新增的任何函数式接口都不允许抛出受检异常（Checked Exception）。

如果需要 Lambda 表达式抛出异常：

- 自定义函数式接口
- try-catch Checked Exception 后再抛出 unchecked exception

### Predicate

记忆：断言（true or false）

在需要表示一个涉及类型 T 的布尔表达式时，就可以使用这个接口

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

### Consumer

记忆：消费（”吃葡萄，不吐葡萄皮“）

需要访问类型T的对象，并对其执行某些操作时，就可以使用这个接口

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

### Function

记忆：处理函数

处理一个泛型 T 的对象，并返回一个泛型 R 的对象

```java
@FunctionalInterface
public interface Function<T, R> { 
    R apply(T t);
}
```

## 简写

编译器可以根据类型推断来简化 Lambda 形参类型：

```java
// 没有类型推断
Comparator<Apple> c = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());

// 依据类型推断简化形参
Comparator<Apple> c = (a1, a2) -> a1.getWeight().compareTo(a2.getWeight());
```

当 Lambda 的形参列表只有一个参数时可以省略括号：

```java
List<Apple> greenApples = filter(inventory, a -> "green".equals(a.getColor()));
```

## 捕获变量

Lambda 表达式中可以任意使用当前类中的实例变量和静态变量，但对于局部变量，要么是 `final` 修改的、要么是事实上的 `final` 变量（非 `final` 但只有初始化时赋值了，之后没有赋值过）。

## 方法引用

当 Lambda 方法体仅需单个方法时，可以使用方法引用，编译器会自动完成参数传递。

形式：`类名::方法名`

对于构造函数也可以使用方法引用：`类名::new`

# stream

可以通过声明式编程完成对数据集的处理。

好处：

- 声明式
- 可传 Lambda，代码的灵活性、可维护性高
- 并行

## 流和集合的区别

- 流是按需计算，集合是完整数据结构
- 流使用内部迭代，集合使用外部迭代
- 流只能消费一次

## 流的操作类型

- 创建流
- 修改流元素（中间操作）
- 消费流元素（终端操作）

### 创建流

```java
Stream.of(Object1, Object2, Object3...);
```

在集合对象上调用 `stream()` 创建流。

```java
// 流的数据类型为：Apple元素序列
List<Apple> → stream() → Apple元素序列

// 流的数据类型为：List<String>元素序列
List<List<String>> → stream() → List<String>元素序列
```

### 中间操作

### 终端操作

**匿名函数的类型即函数式接口的类型。**
