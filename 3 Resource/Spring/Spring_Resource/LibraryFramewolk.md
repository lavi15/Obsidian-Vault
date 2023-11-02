
### Library
- **내 코드가 주체**로 일부 다른 만들어진 객체를 가져와서 사용할 떄 외부 객체

### Framewolk
- 이미 갖추어진 환경에서 **내 코드가 수동적**으로 프레임안에 들어가서 동작할 경우

#### Spring
- IoC (Inversion of Control)
	- 객체의 생명 주기에 대해 제 3자(IoC container등)가 주관
- DI (Dependency Injection)
	- 객체를 인스턴스 형태로 주입 받아서 사용
- AOP (Aspect Oriented Programming)
	- 비지니스와 관계 없는 부분을 따로 다른 모듈로 분리(프록시 사용)

#### ORM
- 객체 지향 패러다임과 관계형 DB 패러다임의 불일치
- 이전에는 개발자가 객체의 데이터를 한땀한땀 매핑해서 DB에 저장 및 조회
- ORM 을 사용함으로써 단순 작업을 줄이고 비즈니스 로직에 집중 가능 

#### JPA
- Java 진영의 ORM 기술 표준
- 인터페이스이고, 여러 구현체가 있지만 주로 **Hibernate**를 많이 사용
- Spring에서는 Spring DATA JPA를 제공
- QueryDSL과 조합해서 많이 사용(타입체크, 동적쿼리)
- 주로 사용하는 Entity
	(@Entity, @id, @Colum, @ManyToOne, @OneToMany, @OneToOne, @ManyToMany)
    (✲ @ManyToMany의 경우 일대다, 다대일로 풀어서 하는게 좋음)
