### applicationContext.xml
```jsx
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">  
  
    <context:property-placeholder location="classpath:db.properties" />  
    <context:component-scan base-package="spring.conf" />  
    <context:component-scan base-package="user.bean" />  
    <context:component-scan base-package="user.dao" />  
    <context:component-scan base-package="user.main" />  
</beans>
```

### SpringConfiguration
```jsx
package spring.conf;  
  
import org.apache.commons.dbcp2.BasicDataSource;  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.context.annotation.PropertySource;  
  
@Configuration  
@PropertySource("classpath:db.properties")  
public class SpringConfiguration {  
    @Value("${jdbc.driver}")  
    private String driver;  
    @Value("${jdbc.url}")  
    private String url;  
    @Value("${jdbc.username}")  
    private String username;  
    @Value("${jdbc.password}")  
    private String password;  
  
    @Bean  
    public BasicDataSource dataSource(){
        BasicDataSource basicDataSource = new BasicDataSource();  
        basicDataSource.setDriverClassName(driver);  
        basicDataSource.setUrl(url);  
        basicDataSource.setUsername(username);  
        basicDataSource.setPassword(password);  
        return basicDataSource;  
    }}
```