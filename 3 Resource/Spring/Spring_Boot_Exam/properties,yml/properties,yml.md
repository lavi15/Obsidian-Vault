
application.properties
```properties
#Server  
server.port=8080  
server.servlet.encoding.charset=UTF-8  
server.servlet.encoding.enabled=true  
server.servlet.encoding.force=true  
  
#JSP  
spring.mvc.view.prefix=/WEB-INF/views/  
spring.mvc.view.suffix=.jsp  
  
#Mysql  
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver  
spring.datasource.url=jdbc:mysql://db-iqpea-kr.vpc-pub-cdb.ntruss.com:3306/test?serverTimezone=Asia/Seoul&allowPublicKeyRetrieval=true&useSSL=false  
spring.datasource.username=user  
spring.datasource.password=qwe123!@#  
  
#MyBatis  
mybatis.config-location=classpath:spring/mybatis-config.xml  
mybatis.type-aliases-package=user.bean  
mybatis.mapper-locations=classpath:mapper/**/*.xml  
  
#파일 업로드  
spring.servlet.multipart.max-file-size=5MB
```

application.yml
```yml
#Server  
server:  
  port: 8080  
  servlet:  
    encoding:  
      charset: UTF-8  
  servlet:  
    encoding:  
      enabled: true  
      force: true  
  
#JSP  
spring:  
  mvc:  
    view:  
      prefix: /WEB-INF/views/  
      suffix: .jsp  
#Mysql  
  datasource:  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    url: jdbc:mysql://db-iqpea-kr.vpc-pub-cdb.ntruss.com:3306/test?serverTimezone=Asia/Seoul&allowPublicKeyRetrieval=true&useSSL=false  
    username: user  
    password: qwe123!@#  
#파일 업로드  
  servlet:  
    multipart:  
      max-file-size: 5MB


#MyBatis  
mybatis:  
  config-location: classpath:spring/mybatis-config.xml  
  type-aliases-package: user.bean  
  mapper-locations: classpath:mapper/**/*.xml  
```
