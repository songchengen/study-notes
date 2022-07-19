- 手动模式
- 自动绑定模式
- 注入类型
	- setter方法
	  collapsed:: true
	  首先得有一个bean类，比如UserHolder
	  
	  ```java
	  class UserHolder {
	    private User user;
	    	
	    public void setUser(User user) {
	  	this.user = user;
	    }
	  }
	  ```
		- 通过xml的方式注入
		  ```xml
		  <bean class="com.sce.thinking.spring.ioc.dependency.injection.UserHolder" >
		    <!-- 通过手动的方式给userHolder bean对象注入user依赖 -->
		    <!-- 前提是要有一个叫user的bean -->
		    <property name="user" ref="user" />
		  </bean>
		  ```
		  让后加载xml即可，荣国`XmlBeanDefinitionReader`类可以加载
		- 通过 Annotation的方式注入
		  ```java
		  @Bean
		  public UserHolder userHolder(User user) {
		    UserHolder uh = new UserHolder();
		    uh.setUser(user);
		    return uh;
		  }
		  ```
		  然后通过`AnnotationConfigApplicationContext`类的加载bean，当然这里有个依赖的user bean对象，所以需要加载user bean，可以通过xml或者别的方式
		- 通过Java API的方式注入
		  通过生成一个`BeanDefinition`对象的方式注入依赖
		  ```java
		  public static BeanDefinition createUserHolderBeanDefinition() {
		    BeanDefinitionBuilder beanDefinitionBuilder = BeanDefinitionBuilder.genericBeanDefinition(UserHolder.class);
		    beanDefinitionBuilder.addPropertyReference("user", "user");
		  
		    return beanDefinitionBuilder.getBeanDefinition();
		  }
		  ```
		  ```java
		  AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext();
		  // 注册BeanDefinition
		  applicationContext.registerBeanDefinition("userHolder", createUserHolderBeanDefinition());
		  ```
		  同样的需要先加载user bean
		- 自动绑定的方式注入
		  ```xml
		  <bean class="com.sce.thinking.spring.ioc.dependency.injection.UserHolder"
		      autowire="byName || byType"
		    >
		  </bean>
		  ```
	- 构造器注入
	  collapsed:: true
		- 通过xml的方式
		  ```xml
		  <bean class="com.sce.thinking.spring.ioc.dependency.injection.UserHolder" >
		    <!-- 通过手动的方式给userHolder bean对象注入user依赖 -->
		    <!-- 前提是要有一个叫user的bean -->
		    <constructor-arg name="user" ref="user" />
		  </bean>
		  ```
		- 通过Annotation的方式注入
		  ```java
		  @Bean
		  public UserHolder userHolder(User user) {
		    return new UserHolder(user);
		  }
		  ```
		- 通过Java API的方式注入
		  ```java
		  public static BeanDefinition createUserHolderBeanDefinition() {
		    BeanDefinitionBuilder beanDefinitionBuilder = BeanDefinitionBuilder.genericBeanDefinition(UserHolder.class);
		    // 与setter相比，只是使用的方法不一样而已
		    beanDefinitionBuilder.addConstructorArgReference("superUser");
		  
		    return beanDefinitionBuilder.getBeanDefinition();
		  }
		  ```
		- 自动绑定的方式实现依赖注入
		  ```xml
		  <bean class="com.sce.thinking.spring.ioc.dependency.injection.UserHolder"
		      autowire="constructor"
		    >
		  </bean>
		  ```
	- 字段注入
	  collapsed:: true
		- @AutoWired
		- @Redource
		- @Inject，需要依赖jar包
	- 方法注入
	  collapsed:: true
		- @Autowired
		- @Resource
		- @Inject(可选）
		- @Bean
	- 接口回调
	  collapsed:: true
		- Aware系列接口
		  实现Aware一系列的接口，可以通过回调的方式获取Spring的一些bean 对象，比如通过实现`BeanFactoryAware`接口，获得`BeanFactory`对象
- 限定注入
  collapsed:: true
	- @Qualifier("bean id")注解
	  ```java
	  @Autowired
	  @Qualifier("superUser")
	  private User user;
	  ```
	- @Qualifier 进行逻辑分组
	  ```java
	  @Bean
	  @Qualifier // 分组限定
	  public User user1() {
	    return new User(1l);
	  }
	  
	  @Bean
	  @Qualifier // 分组限定
	  public User user2() {
	    return new User(2l);
	  }
	  
	  @Autowired
	  @Qualifier("specialUser")
	  private User user; // 限定ID位specialUser的 user Bean
	  
	  @Autowired
	  private User user; // 标记了@Primary的user Bean
	  
	  // TODO
	  // 这里实际上并不包含user1和user2，原理是啥我也没搞懂
	  // 但是如果把user1或user2改成静态方法，那么这里就能包含了
	  @Autowired
	  private List<User> allUsers; // 所有的user beans，包含user1和user2
	  
	  @Autowired
	  @Qualifier
	  private List<User> qualifiedUsers; // 加了Qualifier注解限定的user beans，这里只有user1 和user2
	  ```
	- 自定义@Qualifier的派生注解分组
	  比如@LoadBalanced
	-
- 延迟注入
	- 通过ObjectFactory实现延迟注入
	- 通过@Autowired和@Lazy延迟注入
- 依赖注入的源码流程
	- 核心是`DefaultListableBeanFactory:resolveDependency`方法