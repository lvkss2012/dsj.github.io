- 编译设置
artifacts需要添加信息到web-inf输出:

[artifacts](./../assets/img/artifacts.png)

- controller中返回的是jsp页面，而不是内容。如何返回内容

- servlet配置文件里需要jsp文件的相对路径，例如dispatcher-servlet.xml中的配置：
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.dsj"/>  加入自动扫描注册组件

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/> 指定jsp文件相对位置
        <property name="suffix" value=".jsp"/> 指定后缀是jsp的文件
    </bean>
</beans>
```

- Controller注解：spring扫描基于注解的控制器类
