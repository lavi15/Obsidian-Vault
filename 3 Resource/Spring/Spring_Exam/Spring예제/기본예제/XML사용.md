### applicationContext.xml

```jsx
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="<http://www.springframework.org/schema/beans>"
       xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>"
       xsi:schemaLocation="<http://www.springframework.org/schema/beans> <http://www.springframework.org/schema/beans/spring-beans.xsd>">
    <bean id="calcAdd" class="com.sample04.CalcAdd" scope="prototype"></bean>
    <bean id="calcMul" class="com.sample04.CalcMul" scope="prototype"></bean>
</beans>
```

### CalcSpring

```jsx
package com.sample04;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class CalcSpring {

    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        Calc calc = (Calc)applicationContext.getBean("calcAdd");
        calc.calulate(25,36);

        calc = applicationContext.getBean("calcMul", Calc.class);
        calc.calulate(25,36);
    }
}
```

### Constructor 방식
```jsx
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">  
    <constructor-arg ref="dataSource" />  
</bean>
```

### Setter 방식
```jsx
<bean id="userDAOImpl" class="user.dao.UserDAOImpl">  
    <property name="jdbcTemplate" ref="jdbcTemplate" />  
    <property name="userDTO" ref="userDTO" />  
</bean>
```