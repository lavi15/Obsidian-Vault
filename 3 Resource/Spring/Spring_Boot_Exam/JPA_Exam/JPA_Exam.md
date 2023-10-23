
application.properties
```properties
#JPA  
##스키마 생성 - create(기존 테이블이 있으면 삭제 후 생성), update(변경된 부분만 반영)  
spring.jpa.hibernate.ddl-auto=create  
##DDL 생성 시 데이터베이스 고유의 기능을 사용하겠는가?  
spring.jpa.generate-ddl=true  
##api 호출 시 실행되는 sql문을 콘솔에 보여 줄 것인가?  
spring.jpa.show-sql=true  
##사용할 데이터베이스  
spring.jpa.database=mysql  
##Mysql 상세 설정  
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
```

