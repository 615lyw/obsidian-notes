---
date created: 2023-01-03, 22:06:49
date modified: 2023-01-06, 12:33:55
---

# variables

## Declaring Variables

With the var keyword syntax:

`var variablename type = value`

**Note**: You always have to specify either type or value (or both).

With the `:=` sign syntax:

`variablename := value`

**Note**: In this case, the type of the variable is **inferred** from the value (means that the compiler decides the type of the variable, based on the value).

**Note**: It is not possible to declare a variable using `:=` without assigning a value to it.

## Difference Between var and :=

| var                                                              | :=                                |
| ---------------------------------------------------------------- | --------------------------------- |
| Can be used inside and outside of functions                      | Can only be used inside functions |
| Variable declaration and value assignment can be done separately | Variable declaration and value assignment cannot be done separately (must be done in the same line)                                  |

## Variable Declaration Without Initial Value

if you declare a variable without an initial value, its value will be set to the default value of its type. (如果变量没有声明类型且没有赋初始值，则编译报错)

- bool: `false`
- string: `""`
- int: 0

# data types

- bool
- numeric
    - integer
        - Signed integers
        - Unsigned integers
    - float
- string: String values must be surrounded by double quotes

## bool

## numeric

### integer

- The default type for integer is `int`
- If you do not specify a type, the type will be `int

Signed integers: declared with `int` keywords, can store both positive and negative values.

- `int` : size depends on platform, 32 bits or 64 bits
- `int8` : 8 bits
- `int16` : 16 bits
- `int32` : 32 bits
- `int64` : 64 bits

Unsigned integers (size is same as `int`): declared with `uint` keywords, can only store non-negative values.

- `uint`
- `uint8`
- `uint16`
- `uint32`
- `uint64

### float

The float data types are used to store positive and negative numbers with a decimal point.

- The default type for float is `float64`
- If you do not specify a type, the type will be `float64`

kinds:

- `float32` : 32 bits
- `float64` : 64 bits

# constants

Syntax:

`const CONSTNAME type = value`

- Constants can be declared both inside and outside of a function
- Constant names are usually written in uppercase letters

Constant Types:

- Typed constants
- Untyped constants: In this case, the type of the constant is inferred from the value (means the compiler decides the type of the constant, based on the value).

Multiple Constants Declaration

Multiple constants can be grouped together into a block **for readability**.

```go
package main

import ("fmt")

const (
  A int = 1
  B = 3.14
  C = "Hi!"
)

func main() {
  fmt.Println(A)
  fmt.Println(B)
  fmt.Println(C)
}
```

# If/Else

A statement can precede conditionals; any variables declared in this statement **are available in the current and all subsequent branches.**

```go
if num := 9; num < 0 {
    fmt.Println(num, "is negative")
} else if num < 10 {
    fmt.Println(num, "has 1 digit")
} else {
    fmt.Println(num, "has multiple digits")
}
```

没有三元运算符。

# Switch

可用于类型判断。

# package

- A *package* is a collection of source files in the same directory
- 一个 directory 下只能存在一个 package
- 按惯例，package name 和 derectory name 保持一致
- *main* package 中的 `func main()` 是程序开始执行点

# import

- When importing a package, you can use the menbers that this package **export**s
- imports alias feature
    - 实现包别名，简化使用. e.g `fmt.Println` -> `f.Println`
    - 解决包名冲突

# module

(Go module 类似 [[Maven]]，方便管理依赖)

- [modules 命名最佳实践](https://go.dev/doc/modules/managing-dependencies#naming_module)
- 在 module 根目录下存在 *go. mod* 文件，类似 *POM. xml*，其中定义了 *module path* (在 import package 时表示 module root derectory)
- import package 时 package path syntax: `module_path(module_root_derectory)/the_filesystem_path_to_package/package_name`

# GOROOT and GOPATH

GOROOT 即 Go 安装目录，GOPATH 类似本地 MAVEN 仓库。
