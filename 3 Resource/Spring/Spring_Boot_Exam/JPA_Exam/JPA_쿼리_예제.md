
### JPA 쿼리 메소드 예제
```java
// select * from usertable where name like concat('%', value , '%')
public List<UserDTO> findByNameContaining(String value);  
  
// select * from usertable where id like concat('%', value , '%')
public List<UserDTO> findByIdContaining(String value);

```

### JPA 쿼리 어노테이션
```java
@Query("select userDTO from UserDTO userDTO where userDTO.name like concat('%', ?1 , '%') ")  
public List<UserDTO> getUserSearchName(String value);

@Query("select userDTO from UserDTO userDTO where userDTO.id like concat('%', ?1 , '%') ")  
public List<UserDTO> getUserSearchId(String value);


// 변수명으로 입력시 매개변수가 아닌 파라미터 값으로 인입
@Query("select userDTO from UserDTO userDTO where userDTO.name like concat('%', :value , '%') ")  
public List<UserDTO> getUserSearchName(String value);  
  
  
@Query("select userDTO from UserDTO userDTO where userDTO.id like concat('%', :value , '%') ")  
public List<UserDTO> getUserSearchId(String value);
```
