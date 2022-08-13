# SSM

Spring 框架 + SpringMVC + Mybatis 三者的综合。

## 基于 XML 文件

1. 在 pom.xml 文件中引入 Mybatis 依赖以及 Mybatis 对 Spring 的支持包

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.3</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.4</version>
</dependency>
```

2. 在 web.xml 中配置 DispatcherServlet
3. 在 application.xml 中注册 Mybatis 相关组件

sqlSessionFactoryBean 根据 DataSource →  sqlSession → mapper

```xml
<!--注册Mybatis所需数据源datasource-->
<bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/j20_db"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</bean>

<!--注册用于产生sqlSession实例的sqlSessionFactoryBean工厂-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="datasource"/>
</bean>

<!--注册MapperScannerConfigurer扫描包配置类，用于实现mapper注册-->
<!--作用相当于：<package name="com.cskaoyan.mapper"/>-->
<bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.cskaoyan.mapper"/>
    <!--配置sqlSessionFactory-->
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
</bean>
```

4. **==核心：==**引入 Mybatis 后之前的 controller、service、dao 中的 dao 变成了 mapper，UserDao 变成了 UserMapper 映射器接口，resources 下的 UserMapper.xml 映射文件替换了 UserDaoImpl

5. 其他内容比如组件注册以及依赖注入和 Spring 阶段学的一样

## 基于 JavaConfig

用 Java 类替换两个文件：

- 用继承自 AACDSI 的 ApplicationInitializer 替代 web.xml
- 用 SpringConfig 和 SpringMVCConfig 配置类来替代 application.xml 完成组件的注册与依赖注入

可以看到大部分是配置 sevlet (DispatcherServlet 和 Filter) 的方法，用于替代 web.xml 文件

将 Spring 相关组件的配置和 SpringMVC 相关组件所属的配置类分开来（**使用 springBoot 时不再需要该类**）

```java
public class ApplicationInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    /**
     * 指定Spring父容器的配置类
     */
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[] {SpringConfig.class};
    }

    /**
     * 指定SpringMVC配置类，用于配置Servlet
     */
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[] {SpringMVCConfig.class};
    }

    /**
     * 配置dispatcherServlet的servlet-mapping
     */
    @Override
    protected String[] getServletMappings() {
        return new String[] {"/"};
    }

    /**
     * 配置Filter
     */
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("utf-8");
        filter.setForceEncoding(true);
        return new Filter[] {filter};
    }
}
```

**Spring 和 SpringMVC 的关系：**

Spring 是父容器，SpringMVC 是子容器，Spring 父容器中注册的 Bean 对 SpringMVC 子容器是可见的，反之则不行。

**SpringMVC 和 Spring 容器各自负责注册哪些 bean？**

SpringMVC 主要就是为我们构建 web 应用程序，那么 SpringMVC 子容器用来注册 web 组件的 Bean，如 controller、handlerMapping、viewResolve 等。而 Spring 用来注册驱动应用后端的中间层和数据层组件的 bean，例如 Mybatis 相关组件。

Spring 配置类：

```java
@Configuration
@ComponentScan(value = "com.cskaoyan",
        excludeFilters = @ComponentScan.Filter(type=FilterType.ANNOTATION, value={Controller.class, EnableWebMvc.class}))
@EnableTransactionManagement
@EnableAspectJAutoProxy
public class SpringConfig {

    //datasource
    @Bean
    public DruidDataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/j20_db?useUnicode=true&characterEncoding=utf-8");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }

    //sqlSessionFactoryBean
    @Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        return sqlSessionFactoryBean;
    }

    //MapperScannerConfigurer
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer configurer = new MapperScannerConfigurer();
        configurer.setBasePackage("com.cskaoyan.mapper");
        configurer.setSqlSessionFactoryBeanName("sqlSessionFactoryBean");
        return configurer;
    }

    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }

}
```

SpringMVCConfig 配置类：

```java
@EnableWebMvc
@ComponentScan(value = "com.cskaoyan.controller")
public class SpringMVCConfig implements WebMvcConfigurer {

    /**
     * mvc:resources mapping location
     * @param registry
     */
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/pic1/**").addResourceLocations("file:d://spring/pic/");
        registry.addResourceHandler("/pic2/**").addResourceLocations("classpath:/pic/");
    }

    /**
     * mvc:interceptors
     *     bean
     *     mapping:
     * @param registry
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new CustomInterceptor());
        registry.addInterceptor(new CustomInterceptor2()).addPathPatterns("/user/**");
    }

    @Bean
    public MultipartResolver multipartResolver(){
        return new CommonsMultipartResolver();
    }

    @Bean
    public CustomHandlerExceptionResolver customHandlerExceptionResolver(){
        return new CustomHandlerExceptionResolver();
    }

    @Autowired
    ConfigurableConversionService conversionService;
    @PostConstruct
    public void addConverters(){
        conversionService.addConverter(new String2DateConverter());
    }
    
    @Bean
    @Primary
    public ConfigurableConversionService conversionService(){
        return conversionService;
    }

    @Override
    public Validator getValidator() {
        LocalValidatorFactoryBean localValidatorFactoryBean = new LocalValidatorFactoryBean();
        localValidatorFactoryBean.setProviderClass(HibernateValidator.class);
        return localValidatorFactoryBean;
    }
}
```
