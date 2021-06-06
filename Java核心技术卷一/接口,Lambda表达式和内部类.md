## 接口

### 接口概念

接口(*interface*):主要用来描述类具有什么功能,并不给出具体实现,一个类可以实现(*implement*)一个或多个接口.

接口不是类,而是对类的一组需求描述,这些类要遵从接口描述的统一格式进行定义.

接口中所有方法自动地属于*public*,因此,在接口中声明方法时,不需要提供关键字*public*.

声明某个类需要实现某个接口,使用 ***implements*** 关键字.**ex**:`class Employee implements Comparable`.

虽然在声明接口时无需声明为**public**,但是在实现接口时,必须把方法声明为**public**.

接口不是类,所以不能实例化一个接口,但是可以声明一个接口的变量.`Comparable x; //OK`这个接口变量必须引用实现了接口的类对象.

类可以被继承,接口也可以被扩展.同样使用**extends**关键字.另外接口不能包含实例域或静态方法,但可以包含常量.

``` java
public interface Powered extends Moveable{
    double milesPerGallon();
    double SPEED_LIMIT = 95;
    //接口中的域被自动设为 public static final.
}
```

### 接口与抽象类

- 设计的目的不同.继承是“is-a”,接口更像“have-a”.
- 接口只能对方法进行声明,抽象类即可以声明也可以实现.
- 一个类可以实现多个接口,但只能继承一个类.

### 默认方法

可以为接口的方法提供一个**默认实现**,使用**default**修饰符标记.

``` java
public interface Comparable<T>{
    default int compareTo(T other){return 0;}
}
```

如果一个接口中默认方法与实现该方法的类的超类中方法冲突,遵循**超类优先**原则.

如果一个类实现的多个接口中有同一名称的默认方法,应当如下处理

```java
class Student implements Person,Named{   //Person,Named both have function "getName".
    public String getName(){ return Person.super.getName(); }
}
```

### 对象克隆

如果对一个类使用clone方法,可以使用**.clone()**方法.

但是如果对象含有一个对子对象的引用,原对象和克隆就会共享子对象.这时应该重新定义clone方法.

## lambda表达式

lambda表达式,是一个可传递的代码块.

它的基本格式是 *(参数) -> {表达式内容}*.

对于一个函数式接口(functional interface,只有一个抽象方法的接口),可以提供一个lambda表达式.

## 内部类(inner class)

使用内部类的情况

- 内部类方法可以访问该类定义所在的作用域中的数据,包括private.
- 内部类可以对同一个包中的其他类隐藏.
- 当要定义一个回调函数,使用匿名(anonymous)内部类更为便捷.

内部类既可以访问自身的数据域,也可以访问创建它的外围类对象的数据域.

### 局部内部类

可以在方法中定义一个局部类,它的作用域被限定在声明的块中.

### 匿名内部类

``` java
new superType(construction paramters){
    inner class methods and data
}
```

匿名类没有类名,也不能有构造器.
