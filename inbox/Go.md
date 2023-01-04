---
date created: 2023-01-03, 22:06:49
date modified: 2023-01-03, 22:40:16
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

# modules and packages

Go code is grouped into packages, and packages are grouped into modules. 

Your module specifies dependencies needed to run your code, including the Go version and the set of other modules it requires.

(Go Modules 类似 [[Maven]]，方便管理依赖)

[modules 命名最佳实践](https://go.dev/doc/modules/managing-dependencies#naming_module)

Remember that package names carry most of the weight of describing functionality. The module path creates a namespace for those package names.

**In Go, code executed as an application must be in a main package.**

