## NamedParameterJdbcDaoSupport 이용
**※NamedParameterJdbcDaoSupport 이용 시 Dao 마다 JdbcTemple 생성할 필요가 없음**
### applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">  
        <property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />  
        <property name="url" value="jdbc:oracle:thin:@localhost:1521:xe" />  
        <property name="username" value="c##java" />  
        <property name="password" value="1234" />  
    </bean>  
  
    <bean id="helloSpring" class="user.main.HelloSpring" />  
    <bean id="userDTO" class="user.bean.UserDTO"></bean>  
    <bean id="userDAOImpl" class="user.dao.UserDAOImpl">  
        <property name="userDTO" ref="userDTO" />  
        <property name="dataSource" ref="dataSource"/>  
    </bean>  
    <bean id="userInsertService" class="user.main.UserInsertService">  
        <property name="userDAO" ref="userDAOImpl" />  
        <property name="userDTO" ref="userDTO" />  
    </bean>  
    <bean id="userSelectService" class="user.main.UserSelectService">  
        <property name="userDAO" ref="userDAOImpl" />  
    </bean>  
    <bean id="userUpdateService" class="user.main.UserUpdateService">  
        <property name="userDAO" ref="userDAOImpl" />  
    </bean>  
    <bean id="userDeleteService" class="user.main.UserDeleteService">  
        <property name="userDAO" ref="userDAOImpl" />  
    </bean>  
</beans>
```

### DAO(CURD)
```java
package user.dao;  
  
import lombok.Setter;  
import org.springframework.jdbc.core.BeanPropertyRowMapper;  
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcDaoSupport;  
import user.bean.UserDTO;  
  
import java.util.List;  
import java.util.Map;  
  
public class UserDAOImpl extends NamedParameterJdbcDaoSupport implements UserDAO{  
    @Setter  
    private UserDTO userDTO;  
  
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
    }}
```


## JdbcTemplate 이용
### applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">  
        <property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />  
        <property name="url" value="jdbc:oracle:thin:@localhost:1521:xe" />  
        <property name="username" value="c##java" />  
        <property name="password" value="1234" />  
    </bean>  
  
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">  
        <constructor-arg ref="dataSource" />  
    </bean>  


    <bean id="helloSpring" class="user.main.HelloSpring" />  
    <bean id="userDTO" class="user.bean.UserDTO"></bean>  
    <bean id="userDAOImpl" class="user.dao.UserDAOImpl">  
        <property name="jdbcTemplate" ref="jdbcTemplate" />  
        <property name="userDTO" ref="userDTO" />  
    </bean>  
    <bean id="userInsertService" class="user.main.UserInsertService">  
        <property name="userDAO" ref="userDAOImpl" />  
        <property name="userDTO" ref="userDTO" />  
    </bean>  
    <bean id="userSelectService" class="user.main.UserSelectService">  
        <property name="userDAO" ref="userDAOImpl" />  
    </bean>  
    <bean id="userUpdateService" class="user.main.UserUpdateService">  
        <property name="userDAO" ref="userDAOImpl" />  
    </bean>  
    <bean id="userDeleteService" class="user.main.UserDeleteService">  
        <property name="userDAO" ref="userDAOImpl" />  
    </bean>  
</beans>
```

### DAO(CURD)
```java
package user.dao;  
  
import lombok.Setter;  
import org.springframework.jdbc.core.BeanPropertyRowMapper;  
import org.springframework.jdbc.core.JdbcTemplate;  
import user.bean.UserDTO;  
  
import java.util.List;  
import java.util.Map;  
  
public class UserDAOImpl implements UserDAO{  
    @Setter  
    private JdbcTemplate jdbcTemplate;  
    @Setter  
    private UserDTO userDTO;  
  
    @Override  
    public int insert(UserDTO userDTO) {  
        String sql = "insert into usertable values(?,?,?)";  
        int su = jdbcTemplate.update(sql, userDTO.getName(), userDTO.getId(), userDTO.getPwd());  
        return su;  
    }  
    @Override  
    public List<UserDTO> select() {  
        String sql = "select * from usertable";  
        List<UserDTO> dtoList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<UserDTO>(UserDTO.class));  
        return dtoList;  
    }  
    @Override  
    public int delete(Map<String, String> userDeleteMap) {  
        String sql = "delete usertable where id = ? and pwd = ?";  
        int su = jdbcTemplate.update(sql, userDeleteMap.get("deleteId"), userDeleteMap.get("deletePwd"));  
        return su;  
    }  
    @Override  
    public int update(Map<String, String> userUpdateMap) {  
        String sql = "update usertable set pwd = ? where id = ? and pwd = ?";  
        int su = jdbcTemplate.update(sql, userUpdateMap.get("updatePwd"), userUpdateMap.get("id"), userUpdateMap.get("pwd"));  
        return su;  
    }}
```

