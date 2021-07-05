# Java

## 接口的实例化

经常在Spring中可以看到

```java
//interface A
A a = new B();
```

貌似是对于接口A实例化,实际上,如果类B实现了A,这种写法是符合规范的.

除此之外          

```java
 Ob.fun(new A(){
 	
 	@Override
 	public void onClick(int x){
 		
 	}
 });
```

这种写法是一种匿名内部类的写法,其中`new`产生一个实现接口的匿名内部类.





# Spring

## @Bean和@Component的区别

### 作用

- @Bean 告诉Spring该方法返回一个对象,并将其注册为bean.一般在配置类@Configuration中使用.
- @Component 将一个类作为组件类,并为其创建bean

### 理解

@Component作用于类,Spring会自动扫描并将所有@Component注解类添加到上下文.

如果并不需要该类中所有方法都注册为bean,使用@Bean将方法注册.

两者都可以使用@Autowired装配.











