# 面向切面编程 AOP

以前系统级服务代码（如事务、日志等）和组件的核心业务代码是耦合在一起的，比如很多对象里很多的方法都需要加上事务处理代码，那么我们不得不在每一个方法代码处都添加处理事务的类似代码，造成了“代码冗余”（ [[代码冗余的缺点]] ），修改业务代码时也要小心不要改到事务的代码，事务代码要变化的话也需要到每一处事务代码那进行修改，造成这种难以维护的局面的根本原因在于：

- 事务代码和业务代码紧密耦合在了一起
- 需要添加事务代码的业务点太多了，且分散于整个系统中

有什么方式可以在一个地方进行添加、修改便完成对各处业务代码事务代码的更新？

解决方案为：面向切面编程。AOP 把系统级服务代码（如事务、日志等）和每个组件的业务代码进行解耦（AOP 也是一项 [[解耦技术]] ），即完成了对业务代码的增强，而又没有修改原有代码，符合 [[开放-封闭原则 OCP]]。

AOP 是通过 [[动态代理]] 机制来实现的。
