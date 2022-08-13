# JavaBeans 是什么？

> The goal of the JavaBeans APIs is to define a software component model for Java.
> Since JDK 1.1

# JavaBean 是什么？

> “A Java Bean is a reusable software component that can be manipulated visually in a builder tool.”

最初 JavaBeans 技术主要是为 GUI 服务的，JavaBean 组件类似前端**组件**（比如类似一个按钮，一个输入框等），可以通过构建器创建（构建器类似低代码工作台，通过拖拽生成前端小组件）。

JavaBean 典型特征：

- 支持“内省”，以便构建器分析 bean
- 支持自定义：可以自定义 bean 的行为和外观
- 支持属性
- 支持持久化：通过构建器完成自定义的 bean 可以持久化保存，以便下次直接加载
- 支持事件：事件作为给 bean 传递消息的机制

为了给构建器工具提供操作 bean 的统一方式，有一套 JavaBean 规范：

- 必须有一个 `public` 无参构造器
- 通过符合一定的命名规范的访问器来读写属性字段（getter/setter）
    - `xyz` 对应的读方法：`getXyz()`，对应的写方法：`setXyz()`。`boolean` 字段的读方法一般命名为 `isXyz()`。

# JavaBean 作用

由上面可知 JavaBean 最初设计是为了作为 GUI 组件，现在后端环境中主要用来把一组数据组装成一个 JavaBean 便于传递数据。

# JavaBean 中的一些关键类和方法

- 一切操作 bean 的起点：`Introspector.getBeanInfo()`
- MethodDescriptor
- PropertyDescriptor
    - PropertyEditor
    - getReadMethod
    - getWriteMethod
    - getPropertyType

# 参考

Javabeans 规范： [[beans.101.pdf]]
