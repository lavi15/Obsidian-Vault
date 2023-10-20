**※ 웹과 상관이 없을시 파일 명**
applicationContext.xml

**※ 웹과 상관이 있을 시 파일 명**
/WEB-INF/서블릿이름-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver" >  
        <property name="prefix" value="/WEB-INF/" />  
        <property name="suffix" value=".jsp" />     <!-- 확장자 -->  
    </bean>  
  
    <bean id="sumController" class="com.controller.SumController"></bean>  
    <bean id="sungJukController" class="com.controller.SungJukController"></bean>  
  
</beans>
```