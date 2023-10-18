
**Spring에서의 DAO 구현체 생략**

1. root-context.xml
``` xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd  
                           http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring.xsd">  

    <mybatis-spring:scan base-package="user.dao" />  
  
</beans>
```

2. SpringConfiguration.java
``` java
@MapperScan("user.dao")  
public class SpringConfiguration {}
```