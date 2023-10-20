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
       xmlns:context="http://www.springframework.org/schema/context"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">  
  
    <context:property-placeholder location="classpath:db.properties" />  
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">  
        <property name="driverClassName" value="${jdbc.driver}" />  
        <property name="url" value="${jdbc.url}" />  
        <property name="username" value="${jdbc.username}" />  
        <property name="password" value="${jdbc.password}" />  
    </bean>  
  
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean" >  
        <property name="configLocation" value="classpath:mybatis-config.xml" />  
        <property name="mapperLocations" value="classpath:mapper/userMapper.xml" />  
        <property name="dataSource" ref="dataSource" />  
    </bean>  
  
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" >  
        <constructor-arg ref="sqlSessionFactory" />  
    </bean>  
  
    <bean id="dataSourceTransaction" class="org.springframework.jdbc.datasource.DataSourceTransactionManager" >  
        <constructor-arg ref="dataSource" />  
    </bean>  
  
    <bean id="userDAOMybatis" class="user.dao.UserDAOMybatis" >  
        <property name="sqlSessionFactory" ref="sqlSessionFactory" />  
    </bean>  
  
    <bean id="helloSpring" class="user.main.HelloSpring" />  
    <bean id="userDTO" class="user.bean.UserDTO"></bean>  
    <bean id="userInsertService" class="user.main.UserInsertService">  
        <property name="userDAO" ref="userDAOMybatis" />  
    </bean>  
    <bean id="userSelectService" class="user.main.UserSelectService">  
        <property name="userDAO" ref="userDAOMybatis" />  
    </bean>  
    <bean id="userUpdateService" class="user.main.UserUpdateService">  
        <property name="userDAO" ref="userDAOMybatis" />  
    </bean>  
    <bean id="userDeleteService" class="user.main.UserDeleteService">  
        <property name="userDAO" ref="userDAOMybatis" />  
    </bean>  
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

### userDAO.java
```java
package user.dao;  
  
import lombok.Setter;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.springframework.transaction.annotation.Transactional;  
import user.bean.UserDTO;  
  
import java.util.List;  
import java.util.Map;  
  
@Transactional   //commit(), close()를 해줌  
public class UserDAOMybatis implements UserDAO {  
    @Setter  
    private SqlSessionFactory sqlSessionFactory;  
  
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
