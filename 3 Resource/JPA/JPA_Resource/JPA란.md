
#### JPA란?? 
- JAVA 진영의 ORM 표준
- Object-relational mapping(객체 관계 매핑)

#### JPA 실무에서 어려운 이유
- 객체와 테이블을 매핑을 잘하지 못하기 때문
- 객체와 테이블을 제대로 설계하고 매핑하는 방법
- 기본 키와 외래 키 매핑
- 1:N, N:1, 1:1, N:M 매핑
- JPA의 내부 동작 방식을 제대로 이해하지 못하기 때문

#### JPA를 사용해야 하는 이유
- SQL 중심적인 개발에서 객체 중심적으로 개발
- 생산성 증가
	- 저장 :  jpa.persist(member)
	- 조회 :  Memeber member =jpa.find(memberId)
	- 수정 : member.setName("변경할 이름")
	- 삭제 : jpa.remove(member)
- 유지보수 용이
- 성능

#### JPA의 동작
- 저장 
![[Pasted image 20231117231049.png]]
- 조회
![[Pasted image 20231117231111.png]]

#### JPA와 비교하기
Member member1 = jpa.find(Member.class, memberId);
Member member2 = jpa.find(Member.class, memberId);

member1 == member2;  **true**
**JPA는 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장**

## JPA 성능 최적화 기능
#### 1차 캐시와 동일성 보장
- 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장 - 약간의 조회 성능 향상
- DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read 보장

#### 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
- 트랜잭션을 커밋할 떄까지 insert sql을 모음
- jdbc batch sql 기능을 사용해서 한번에 SQL 전송
```java
transaction.begin();   //트랜젝션 시작
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);
transaction.commit();  //트랜잭션 커밋하는 순간 sql문 3개를 보냄
```

#### 지연로딩(Lazy Loading)
- UPDATE,DELETE로 인한 ROW락 시간 최소화
- 트랜잭션 커밋 시 UPDATE, DELETE SQL 실행하고, 바로 커밋

## 지연 로딩과 즉시 로딩
- 지연 로딩 : 객체가 실제 사용될 때 로딩
- 즉시 로딩 : JOIN SQL로 한번에 연관된 객체까지 미리 조회

### 데이터베이스 방언
- 특정 데이터베이스에만 존재하는 문법