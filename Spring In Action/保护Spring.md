# 保护Spring

## 使用Spring Security

通过添加security依赖以使用

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

添加该依赖之后

- 所有HTTP请求路径都需要认证
- 不需特定角色和权限
- 没有登录界面
- 认证过程通过HTTP basic认证对话框实现
- 系统仅有用户user

## 配置Spring Security

对于用户存储配置,Spring Security有多个可选方案

- 基于内存的用户存储

  适用于用户数量有限且几乎不会发生改变

- 基于JDBC的用户存储

- 以LDAP(Lightweight Directory Access Protocol,轻量级目录访问协议)为后端的用户存储

- 自定义用户详情服务

