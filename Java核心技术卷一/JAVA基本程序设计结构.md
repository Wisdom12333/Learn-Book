# JAVA基本程序设计结构

### 基本格式

``` java
public class ClassName{
    public static void main(String[] args){
        //Statement
    }
}
```

#### Java 的方法调用

> $ object.method(parameters)$

#### 三种注释

- 使用 `//` 进行单行注释.
-  `/* */`进行段注释.
- `/**  */`用于自动生成文档.

### 数据类型

##### 整型

-   int       4字节                         -2 147 483 648 ~ 2 147 483 647
- short    2字节                                      -32 768 ~ 32 767
-  long     8字节  -9 223 372 036 854 775 808 ~ 9 223 372 036 854 775 807
-  byte     1字节                                           -128 ~ 127

使用后缀**L**或**l**表示长整型,前缀**0x**或**0X**表示十六进制数值.前缀**0b**或**0B**表示二进制数.

可以为数字加下划线以便于阅读(0b1_0001_0001).

##### 浮点型

-   float    4字节  约 ± 3.402 823 47E+38F (有效位数6~7位)
- double  8字节  约 ± 1.797 693 134 862 315 70E+308 (有效位数15位)

使用常量**Double.POSITIVE_INFINITY,Double.NEGATIVE_INFINITY**和**Double.NaN**分别表示**正无穷大,负无穷大**和**NaN(不是一个数字,Not a Number)**.

使用` Double.isNaN(x) `方法判断是否为NaN.

##### char

表示字符.

##### boolean类型

false 和 true , 判断逻辑条件,与C++不同,不能与整型相互转换.

可以通过 `x?1:0`来将整型转换为布尔类型

##### 常量和变量

使用 final 指示常量,习惯上使用全大写表示.

`final int TOTAL_GRADE = 100;`

如果希望常量可以在一个类的多个方法使用,可以使用`static final`.

### 控制流程

使用`{}` 将若干语句组合为一个块(block),与C++不同,Java不允许在一个嵌套的块中重新定义一个变量.

##### 中断

Java提供一种带标签的`break`语句.用于跳出多重嵌套的循环.

``` java
label:
while(...){
    for(...){
        if(...)break label;
    }
}
//goto here after the "label" break
```

### 数组

使用`int[] a = new int[100]`创建数组.

通过 **for each**循环来遍历

`for (variable : collection) statement`

``` java
for(int element : a)
    System.out.println(element);
//打印数组中每一个元素
```

`int [] a = {1,2,3,4,5};`可以创建数组的同时进行初始化.

如果想对一个数组重新初始化,可以使用

`a = new int[] {2,3,4,5};`

数组的长度可以为0,并且与空串(null)不同.

想要增加数组大小,可以使用**copyOf**方法,也用来拷贝数组:

拷贝: `int[] copiedA = Arrays.copyOf(a , a.length);`

增加长度: `a = Arrays.copyOf(a, 2 * a.length);`,多余的元素将被赋值为0(数值数组),false(布尔数组).