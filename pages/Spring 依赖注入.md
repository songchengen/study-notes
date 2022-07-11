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
		- Aware系列接口
		  |    内建接口   |   说明    |
		  |    ------        |    ------ |
		  | BeanFactoryAware| 获取IoC容器 - BeanFactory |
		-
		-
-