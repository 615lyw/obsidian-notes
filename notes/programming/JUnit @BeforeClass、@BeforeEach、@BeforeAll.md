Meta:

- Parents: [[JUnit]]
- Status: #Update 
- Refs: [@Before vs @BeforeClass vs @BeforeEach vs @BeforeAll | Baeldung](https://www.baeldung.com/junit-before-beforeclass-beforeeach-beforeall)

---

JUnit 4 中 @BeforeClass 和 @AfterClass：

- 在所有 @Test 方法执行前后执行
- 方法必须为静态方法，如：`public static void method()`

在 JUnit 5 中对注解进行了重命名：@Before 对应 @BeforeEach，@BeforeClass 对应 @BeforeAll.