# Reference

- [官方文档](http://shiro.apache.org/documentation.html)
- [跟我学 Shiro](https://doc.yonyoucloud.com/doc/wiki/project/shiro/index.html)

# Shiro

- Shiro 是开源的、声明式的安全框架
- 特性：
    - 认证：身份认证
    - 授权：权限验证，验证某个已认证的用户是否拥有某个权限
    - 加密：密码加密存储至数据库
    - 会话管理：用户登录后产生的所有信息可以放至会话中，用户退出后即清除
    - 缓存：缓存用户信息，角色，权限，不用多次查询数据库
    - Web 集成
- Shiro VS Spring Security
    - Shiro 更小巧简单
    - Spring Security 更强大

# Shiro 框架的内外部视角

## Shiro 外部使用者视角

![](https://atts.w3cschool.cn/attachments/image/wk/shiro/2.png)

- Subject：当前用户
    - **Shiro 对外核心 API**
    - 可以理解为一个门面，实际会委托给 SecurityManager 执行
- SecurityManager：安全管理器
    - Shiro 核心组件
    - 管理所有用户
    - 可以理解为 DispatcherServlet
- Realm：
    - 给 Shiro 提供安全数据（用户信息、角色、权限）
    - 访问数据库获取安全数据
    - 可以理解为 DataSource

综上：应用代码通过 Subject 进行认证与授权，Subject 委托给 SecurityManager，SecurityManager 从 Realm 中获得安全数据进行安全性判断

## Shiro 内部组件体系

![](https://atts.w3cschool.cn/attachments/image/wk/shiro/3.png)

 - **Authenticator**：认证器，负责主体的认证，可拓展
 - **Authorizer**：授权器，负责主体的授权
 - SessionManager：抽象出来用于管理主体与应用之间交互的数据，使之不仅可以用在 Web 环境，还可以用于 SE 环境
 - SessionDAO：Session 存入数据库
 - CacheManager：缓存控制器
 - Cryptography：密码模块，提供加密解密功能

# 认证

认证即验证 Subject 是否为系统中合法用户。是授权的前一步。

- **principals**：主体的唯一标识，例如用户名
- **credentials**：主体的密码、安全凭证

`subject.login(new UsernamePasswordToken(username, password))`

`subject.isAuthenticated()`

subject.login(AuthenticationToken) 认证过程中的核心方法和类：

1. ModularRealmAuthenticator 类：比对 token 中的密码和由 realm 传来的系统中的密码
    - ==AuthenticationInfo doAuthenticate(AuthenticationToken)==：在该方法中调用 realm 对象

2. **realm 对象**：根据 token 中的用户名去取存储在系统中密码交给认证器
    - ==AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken)==：后续我们自己要实现的方法，根据 token 中的用户名获取存储在系统中密码

核心流程：

ModularRealmAuthenticator → 调用 realm → realm 根据传入的 token 中的 username 去用户表或文件中查找用户是否存在，生成 authenticationInfo 返回给 ModularRealmAuthenticator：

- 若不存在，authenticationInfo 为 null；抛出用户不存在异常
- 若存在，authenticationInfo 中有用户表中的 username 和 password， ModularRealmAuthenticator 检查 token 中的用户密码是否与 authenticationInfo 中的密码一致；抛出密码不正确异常

# 授权

权限写法：user:delete

角色与权限的关系：多对多

role1=user:delete

基于角色判断：

hasRole()

hasRoles()

hasAllRoles()

基于权限判断：

isPermitted()

# Web 集成

Shiro 通过 ShiroFilter 拦截**需要安全控制的 URL** 进行安全控制，是整个**安全控制的入口**，通过**读取 URL 配置判断 URL 是否需要登录/授权**。

# 拦截器机制

# Subject

- Subject 是安全领域的术语，表示访问应用的当前用户
- 不取名为 User 仅因为已经有很多其他框架都叫 User
- Subject 类封装了安全状态的属性以及相关操作
    - authentication (login)
    - authorization (access control)
    - session access
    - logout
- 获取当前用户（当前线程）
    - `Subject currentUser = SecurityUtils.getSubject();`
    - 在 web 环境下，Subject 和当前请求线程绑定
- 大多数情况下通过 `SecurityUtils.getSubject();` 获取当前用户。
- 什么特殊情况下需要 custom subject？
    - System startup/bootstrap
    - Integration Testing
    - Daemon/background process work
