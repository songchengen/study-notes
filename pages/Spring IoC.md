- 概述
  控制反转（IoC）在程序设计中，是一种设计模式（原文是 programming principle ）。在IoC编程中，由通用的框架程序控制用户自定义程序，我们不必再面对繁重的生命周期和复杂的依赖关系，专心编写核心业务代码。
- ClassPathXmlApplicationContext
  通过class path下xml的方式加载配置文件的类，现在基本不适用了。类似功能的还有AnnotationConfigApplicationContext，通过注解的形式加载配置类
- ApplicationContext
  ApplicationContext启动的时候，会负责创建bean实例，向各个实例中注入依赖
  他提供的一个较为底层的接口，实现了ListableBeanFactory和HierarchicalBeanFactory接口