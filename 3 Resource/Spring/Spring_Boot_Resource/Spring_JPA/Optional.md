
[[JPA_Exam]]

- Optional은 Java 8에 추가된 새로운 API로 이전에 하던 '고통스러운 null 처리'를 '잘' 다룰 수 있게 도와주는 클래스
- Optional이란 'null일 수도 있는 객체'를 감싸는 일종의 Wrapper 클래스

### optional
```java
Optional<T> optional
```
- optional 변수 내부에는 null이 아닌 T 객체가 있을 수도 있고 null이 있을 수도 있음
- Optional 클래스는 여러 가지 API를 제공하여 null일 수도 있는 객체를 다룰 수 있도록 도움 
- Optional은 값을 Wrapping하고 다시 풀고, null일 경우에는 대체하는 함수를 호출하는 등의 오버헤드가 있으므로 성능이 저하될 수 있음
- 그렇기 때문에 메소드의 반환 값이 절대 null이 아니라면 Optional을 사용하지 않는 것이 좋음


### findById(ID id)
```java
Optional<T> findById
```
- CrudRepository의 findById는 Entity의 id로 검색하여, Optional를 리턴 타입으로 반환하는 메서드
- isPresent()로 Optional 내부에 객체가 들어있는지 확인 후 get()을 이용해 반환된 객체를 사용 가능

### 예제
```java
Optional<ExEntity> optional = exRepsoitory.findById(id); if(optional.isPresent()) { ExEntity exEntity = optional.get(); } else { throw new Exception(); }
```
- orElse(...)에서 ...는 **Optional에 값이 있든 없든 무조건 실행**
- ...가 새로운 객체를 생성하거나 새로운 연산을 수행하는 경우에는 orElse() 대신 orElseGet()을 사용
- Optional에 값이 없으면 orElse()의 인자로서 실행된 값이 반환
- Optional에 값이 있으면 orElse()의 인자로서 실행된 값이 무시되고 버려짐
- orElse(...)는 **...가 새 객체 생성이나 새로운 연산을 유발하지 않고 이미 생성되었거나 이미 계산된 값**일 때만 사용
- orElseGet(...)에서 ...는 **Optional에 값이 없을 때만 실행**
- Optional에 값이 없을 때만 새 객체를 생성하거나 새 연산을 수행하므로 불필요한 오버헤드가 없음