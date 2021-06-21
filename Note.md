# Spring

## @Bean和@Component的区别

### 作用

- @Bean 告诉Spring该方法返回一个对象,并将其注册为bean.一般在配置类@Configuration中使用.
- @Component 将一个类作为组件类,并为其创建bean

### 理解

@Component作用于类,Spring会自动扫描并将所有@Component注解类添加到上下文.

如果并不需要该类中所有方法都注册为bean,使用@Bean将方法注册.

两者都可以使用@Autowired装配.

