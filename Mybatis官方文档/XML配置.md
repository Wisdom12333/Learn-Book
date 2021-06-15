# XML配置

## 属性(properties)

属性可以在外部进行配置,并进行动态替换.

可以在Java文件中配置,也可以在properties元素的子元素设置.

```xml
<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```

设置好的属性可以在整个配置文件中用来替换需要动态配置的属性值.

如果一个属性有多个配置,将按照如下顺序加载

- 首先读取在 properties 元素体内指定的属性。
- 然后根据 properties 元素中的 resource 属性读取类路径下属性文件，或根据 url 属性指定的路径读取属性文件，并覆盖之前读取过的同名属性。
- 最后读取作为方法参数传递的属性，并覆盖之前读取过的同名属性。



从 MyBatis 3.4.2 开始，你可以为占位符指定一个默认值。例如：

```xml
<dataSource type="POOLED">
  <!-- ... -->
  <property name="username" value="${username:ut_user}"/> <!-- 如果属性 'username' 没有被配置，'username' 属性的值将为 'ut_user' -->
</dataSource>
```

这个特性默认是关闭的。要启用这个特性，需要添加一个特定的属性来开启这个特性。例如：

```xml
<properties resource="org/mybatis/example/config.properties">
  <!-- ... -->
  <property name="org.apache.ibatis.parsing.PropertyParser.enable-default-value" value="true"/> <!-- 启用默认值特性 -->
</properties>
```

## 设置(settings)

实例如下

具体设置内容查看[官方文档](https://mybatis.org/mybatis-3/zh/configuration.html)

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
</settings>
```

## 类型别名(typeAliases)

可为Java类型设置一个缩写名字.但仅可用于XML配置.

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
</typeAliases>
```

也可指定包名,Mybatis会在包下搜索需要的Java Bean.

such as

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

该包下的每一个Bean,将使用其首字母小写非限定类名作为别名,另外也可以通过注解手动更改.

```java
@Alias("author")
public class Author {
    ...
}
```

## 类型处理器(typeHandlers)

Mybatis设置预处理语句(PreparedStatement)中的参数或从结果集中取出一个值,会使用类型处理器将获取到的值以合适的方式转换成Java类型.

### 处理枚举类型

若想映射枚举类型 `Enum`，则需要从 `EnumTypeHandler` 或者 `EnumOrdinalTypeHandler` 中选择一个来使用。

比如说我们想存储取近似值时用到的舍入模式。默认情况下，MyBatis 会利用 `EnumTypeHandler` 来把 `Enum` 值转换成对应的名字。

**注意 `EnumTypeHandler` 在某种意义上来说是比较特别的，其它的处理器只针对某个特定的类，而它不同，它会处理任意继承了 `Enum` 的类。**

## 对象工厂(objectFactory)

Mybatis创建结果对象新实例,都会使用一个对象工厂实例来完成.可以创建自己的对象工厂来覆盖默认行为.

## 环境配置(environments)

Mybatis可以配置成适应多种环境.

但是每个SqlSessionFactory实例只能选择一种环境.而每个数据库对应一个SqlSessionFactory实例.

environments元素定义如何配置环境

```xml
<environments default="development">
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
```

- 默认使用的环境 ID（比如：default="development"）。
- 每个 environment 元素定义的环境 ID（比如：id="development"）。
- 事务管理器的配置（比如：type="JDBC"）。
- 数据源的配置（比如：type="POOLED"）。

#### 事务管理器(transactionManager)

Mybatis有两种事务管理器(type),

- JDBC
- MANAGED

> Spring+Mybatis不需要配置事务管理器.

#### 数据源(dataSource)

使用标准JDBC数据源接口配置JDBC连接对象的资源.

有三种内建的数据源类型(type)

##### UNPOOLED

每次请求时打开和关闭连接,性能取决于使用的数据库.只需配置五种属性

- `driver` – 这是 JDBC 驱动的 Java 类全限定名（并不是 JDBC 驱动中可能包含的数据源类）。
- `url` – 这是数据库的 JDBC URL 地址。
- `username` – 登录数据库的用户名。
- `password` – 登录数据库的密码。
- `defaultTransactionIsolationLevel` – 默认的连接事务隔离级别。
- `defaultNetworkTimeout` – 等待数据库操作完成的默认网络超时时间（单位：毫秒）。查看 `java.sql.Connection#setNetworkTimeout()` 的 API 文档以获取更多信息。

##### POOLED

利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间.

##### JNDI

为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的数据源引用。这种数据源配置只需要两个属性

- `initial_context` – 这个属性用来在 InitialContext 中寻找上下文（即，initialContext.lookup(initial_context)）。这是个可选属性，如果忽略，那么将会直接从 InitialContext 中寻找 data_source 属性。
- `data_source` – 这是引用数据源实例位置的上下文路径。提供了 initial_context 配置时会在其返回的上下文中进行查找，没有提供时则直接在 InitialContext 中查找。

## 数据库厂商标识（databaseIdProvider）

Mybatis可以根据不同数据库厂商,执行不同语句.基于映射语句中的`databaseId`属性.MyBatis 会加载带有匹配当前数据库 `databaseId` 属性和所有不带 `databaseId` 属性的语句。 如果同时找到带有 `databaseId` 和不带 `databaseId` 的相同语句，则后者会被舍弃。

## 映射器(mappers)

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

