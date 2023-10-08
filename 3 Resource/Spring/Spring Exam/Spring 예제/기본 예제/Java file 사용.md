### SpringConfiguration
```jsx
package spring.conf;  
  
import org.apache.commons.dbcp2.BasicDataSource;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
  
@Configuration  
public class SpringConfiguration {  
  
    @Bean  
    public BasicDataSource dataSource(){  
        BasicDataSource basicDataSource = new BasicDataSource();  
        basicDataSource.setDriverClassName("oracle.jdbc.driver.OracleDriver");  
        basicDataSource.setUrl("jdbc:oracle:thin:@localhost:1521:xe");  
        basicDataSource.setUsername("c##java");  
        basicDataSource.setPassword("1234");  
        return basicDataSource;  
    }}
```