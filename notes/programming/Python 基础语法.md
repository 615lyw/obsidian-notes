# 模块与包

模块（module）：拓展名为 .py 的代码文件。
包（package）：包含模块的目录，包下必须有一个 `__init__.py` 模块。

故全限定名：package.module.function

## 导入模块

通过 `import` 导入另一个模块即可使用另一模块的类或函数。

别名：`import module_name as alias_name`

导入整个模块：`import module_name as alias_module_name`
使用 `module_name.function()`

导入模块中特定函数：`from module_name import function_1, function_2`
使用 `function()`

# 数据类型与变量

Python 是[[动态语言 VS 静态语言|动态语言]]，其变量无需声明类型也不用写关键字。

整数：

- 没有大小限制

浮点数：

- 没有大小限制，但超出一定范围会显示为 `inf` （无限大）

布尔值：（注意大小写敏感）

- True
- False

空值：`None`

# 运算符

## 大小关系运算符

## 身份运算符

## 成员运算符

## 位运算符

## 逻辑运算符

- `and`
- `or`
- `not`

## 切片运算符

# 流程控制

## 循环

```python
for e in list:
	something()
```

```python
while conditon:
	something()
```

`break`

`continue`

## 条件

```python
if 布尔表达式、非零数值、非空字符串、非空list:
	thing_1()
elif: # elif 是 else if 的缩写
	thing_2()
else: # 最后的 else 属于 elif 的 if
	thing_3()
```

## Switch

# 函数

```python
# 函数定义
def function_name():
	"""函数文档描述"""
	something()

# 函数调用
function_name()
```

返回值不用声明

## 指定形参传值

## 形参默认值（可选实参）

## 可变参数

# 序列

序列中的元素都是有序排列的，**可以通过下标偏移量访问序列的一个或多个元素**。

## 切片运算符

## 字符串

单双引号都行

字符串拼接

## 元组

## 列表

# 字典

记录映射关系的数据结构。

# 推导式

更便捷地生成满足一定规则的列表或字典。

形式：每项表达式 变量一取值范围 变量二的取值范围 条件表达式

## 列表推导式

例子：`[x + y for x in 'AB' for y in '12']` 即 `['A1', 'A2', 'B1', 'B2']`

```python
list = []
for x in 'AB':
	for y in '12':
		list.append(x + y)
```

## 字典推导式

`{i : 0 for i in 'AB'}` 即 `{'A': 0, 'B': 0}`


Python VS Java

字符串可以使用单引号表示，且单双引号混合使用时，只需最外层表示字符串的单引号或双引号成对匹配即可。

`1 / 4` 表达式结果

- Python：0.25
- Java：0