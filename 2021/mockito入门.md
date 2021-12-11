Mockito入门

1. 在低版本中需要使用MockitoAnnotations.initMocks()方法来初始化当前测试类中所有@Mock注解的对象.  
2. final类，匿名类和基本类型无法Mock
3. static和final修饰的方法无法设定when().then()
4. 可以使用verify(mock).xxx()



参考：  
- 简介：https://kucw.github.io/blog/2020/2/spring-unit-test-mockito/ 

- doc：https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html

- issues：https://github.com/mockito/mockito/issues
