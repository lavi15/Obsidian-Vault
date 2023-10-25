
- JPA를 이용해서 목록 기능을 구현할 때는 JPQL(Java Persistence Query Language)을 이용
- JPQL은 검색 대상이 엔티티 라는 것만 제외하고는 기본 구조와 문법이 기존의 SQL과 유사
- 메소드의 이름으로 필요한 쿼리를 만들어주는 기능
- 현재 사용하는 Repository 인터페이스에서 선언된 타입 정보를 기준으로 자동 엔티티 이름이 적용
- 쿼리 메소드의 리턴 타입은 `Page<T>, Slice<T>, List<T>` 이며 모두 `Collection<T>`타입
- 단순히 목록을 검색하려면 `List<T>`를 사용하고 페이징 처리를 하려면 `Page<T>`를 사용

![[Pasted image 20231025102325.png]]
