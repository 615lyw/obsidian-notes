# 简介

运行在浏览器中的解释性、动态编程语言。[[动态语言 VS 静态语言]]

网景开发了JavaScript，用于在静态HTML页面上添加一些动态效果。

ECMAScript 标准，类似于 Java Specification，JavaScript 只是 ECMAScript 标准的一种实现。

# 如何在 HTML 页面中引入 JS 代码？

# 基本语法

不强制要求 `;` 结尾，但最好加上。

JavaScript严格区分大小写

# 数据类型

**Number**

JS 不区分整型和浮点型

两个特殊值：
- NaN
- Infinity

````
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
````

**字符串**
单双引号均可。

**布尔值**

true

false

**逻辑运算符**
&& 与
|| 或
! 非

**比较运算符**
`==`：会自动转换数据类型，不建议使用

`===`：不会自动转换数据类型，如果类型不相同，返回 false，类型一致再比较

`NaN` 与所有其他值都不相等，包括它自己

```
NaN === NaN; // false
```

唯一能判断`NaN`的方法是通过`isNaN()`函数：

```
isNaN(NaN); // true
```

**null 和 undefined**

JavaScript的设计者希望用`null`表示一个空的值，而`undefined`表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用`null`。`undefined`仅仅在判断函数参数是否传递的情况下有用。

**数组**
通过 `[]` 定义，通过下标随机访问。

**对象**
一组由键-值组成的无序集合。

```
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
```

JavaScript对象的键都是字符串类型，值可以是任意数据类型。

要获取一个对象的属性，我们用`对象变量.属性名`的方式：

```
person.name; // 'Bob'
person.zipcode; // null
```

**变量**
通过 `var` 申明一个变量。

使用等号 `=` 对变量进行赋值。可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量。

**strict 模式**
不通过 `var` 申明的变量是全局变量，作用域在：同一个页面中不同JS文件中生效。

建议开启 strict 模式。

# 函数

## 箭头函数
