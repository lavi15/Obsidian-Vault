### mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "https://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <typeAliases>  
        <typeAlias type="user.bean.UserDTO" alias="UserDTO" />  
    </typeAliases>  
  
</configuration>
```

### db.properties
```
jdbc.driver=oracle.jdbc.driver.OracleDriver  
jdbc.url=jdbc:oracle:thin:@localhost:1521:xe  
jdbc.username=c##java  
jdbc.password=1234
```


### applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">  
  
  
    <context:component-scan base-package="user.bean" />  
    <context:component-scan base-package="user.dao" />  
    <context:component-scan base-package="user.main" />  
    <context:component-scan base-package="spring.conf" />  
	  <!--transaction 을 사용할 경우 아래의 설정을 해주어야 함--> 
	  <!--해당 설정을 하지 않을 경우 transaction을 사용하는 class에
	  @EnableTransactionManagement 어노테이션을 써야함--> 
    <tx:annotation-driven transaction-manager="dataSourceTransaction" />  
</beans>
```

### userMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="user">  
    <insert id="insert" parameterType="java.util.Map">  
        insert into usertable values(#{name}, #{id}, #{pwd})  
    </insert>  
    <select id="select" resultType="user.bean.UserDTO">  
        select * from usertable  
    </select>  
    <delete id="delete" parameterType="java.util.Map">  
        delete usertable where id = #{deleteId} and pwd = #{deletePwd}  
    </delete>  
    <update id="update" parameterType="java.util.Map">  
        update usertable set pwd = #{updatePwd} where id = #{id} and pwd = #{pwd}  
    </update>  
</mapper>
```

### SpringConfiguration
```java
package spring.conf;  
  
import org.apache.commons.dbcp2.BasicDataSource;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.mybatis.spring.SqlSessionFactoryBean;  
import org.mybatis.spring.SqlSessionTemplate;  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.context.annotation.PropertySource;  
import org.springframework.core.io.ClassPathResource;  
import org.springframework.core.io.PathResource;  
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;  
import org.springframework.jdbc.datasource.DataSourceTransactionManager;  
  
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
    }  
    @Bean  
    public SqlSessionFactory sqlSessionFactory() throws Exception {  
        SqlSessionFactoryBean sqlSessionFactory = new SqlSessionFactoryBean();  
        sqlSessionFactory.setConfigLocation(new ClassPathResource("mybatis-config.xml"));  
        sqlSessionFactory.setMapperLocations(new ClassPathResource("mapper/userMapper.xml"));  
        sqlSessionFactory.setDataSource(dataSource());  
  
        return sqlSessionFactory.getObject();  
    }  
    @Bean  
    public SqlSession sqlSession() throws Exception {  
        SqlSession sqlSession = new SqlSessionTemplate(sqlSessionFactory());  
        return sqlSession;  
    }  
    @Bean  
    public DataSourceTransactionManager dataSourceTransaction() {  
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager(dataSource());  
        return dataSourceTransactionManager;  
    }  
}
```

### UserDAO
```java
package user.dao;  
  
import lombok.Setter;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Repository;  
import org.springframework.transaction.annotation.Transactional;  
import user.bean.UserDTO;  
  
import java.util.List;  
import java.util.Map;  
  
@Repository  
@Transactional   //commit(), close()를 해줌  
public class UserDAOMybatis implements UserDAO {  
    @Setter  
    @Autowired    private SqlSessionFactory sqlSessionFactory;  
  
    @Override  
    public int insert(Map<String, String> insertMap) {  
        SqlSession sqlSession = sqlSessionFactory.openSession();  
        int su = sqlSession.insert("user.insert", insertMap);  
        return su;  
    }  
    @Override  
    public List<UserDTO> select() {  
        SqlSession sqlSession = sqlSessionFactory.openSession();  
        List<UserDTO> dtoList = sqlSession.selectList("user.select");  
        sqlSession.close();  
        return dtoList;  
    }  
    @Override  
    public int delete(Map<String, String> userDeleteMap) {  
        SqlSession sqlSession = sqlSessionFactory.openSession();  
        int su = sqlSession.delete("user.delete", userDeleteMap);  
        return su;  
    }  
    @Override  
    public int update(Map<String, String> userUpdateMap) {  
        SqlSession sqlSession = sqlSessionFactory.openSession();  
        int su = sqlSession.update("user.update", userUpdateMap);  
        return su;  
    }}
```