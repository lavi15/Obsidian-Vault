### SpringConfiguration.java
```java
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

### DAO.java
```java
package user.dao;  
  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.context.annotation.Scope;  
import org.springframework.jdbc.core.BeanPropertyRowMapper;  
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcDaoSupport;  
import org.springframework.stereotype.Repository;  
import user.bean.UserDTO;  
  
import javax.sql.DataSource;  
import java.util.List;  
import java.util.Map;  
  
  
@Repository  
@Scope("prototype")  
public class UserDAOImpl extends NamedParameterJdbcDaoSupport implements UserDAO{  
  
    @Autowired  
    public void setDs(DataSource dataSource) {  
        setDataSource(dataSource);  
    }  
    @Override  
    public int insert(Map<String, String> insertMap) {  
        String sql = "insert into usertable values(:name, :id, :pwd)";  
        int su = getNamedParameterJdbcTemplate().update(sql, insertMap);  
        return su;  
    }  
    @Override  
    public List<UserDTO> select() {  
        String sql = "select * from usertable";  
        List<UserDTO> dtoList = getNamedParameterJdbcTemplate().query(sql, new BeanPropertyRowMapper<UserDTO>(UserDTO.class));  
        return dtoList;  
    }  
    @Override  
    public int delete(Map<String, String> userDeleteMap) {  
        String sql = "delete usertable where id = :deleteId and pwd = :deletePwd";  
        int su = getNamedParameterJdbcTemplate().update(sql, userDeleteMap);  
        return su;  
    }  
    @Override  
    public int update(Map<String, String> userUpdateMap) {  
        String sql = "update usertable set pwd = :updatePwd where id = :id and pwd = :pwd";  
        int su = getNamedParameterJdbcTemplate().update(sql, userUpdateMap);  
        return su;  
    }  
}
```

### applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">  
  
    <context:component-scan base-package="spring.conf" />  
    <context:component-scan base-package="user.bean" />  
    <context:component-scan base-package="user.dao" />  
    <context:component-scan base-package="user.main" />  
</beans>
```
