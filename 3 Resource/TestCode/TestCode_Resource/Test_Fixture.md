
- @BeforeEach, @BeforeAll은 각 테스트간 의존성이 생기고, 아래에 있는 테스트에서 확인하기 힘듬
- given절에 모두 작성 및 @AfterEach로 매번 클랜징 해주는걸 권고
- @BeforeEach를 써도 되는 경우
	- 각 테스트 입장에서 몰라도 테스트 내용을 이해하는데 문제가 없을 때
	- 수정을 해도 각 테스트에 영향이 전혀 없을 때

### deleteAll vs deleteAllInBatch
- deleteAll
	- **장점** : FK를 맺고 있는 테이블을 다 찾아서 지워줌
	- **단점** : select해서 건건이 테이블을 삭제하기 때문에 쿼리가 많아짐
- deleteAllInBatch
	- **장점** : 특정 테이블을 drop 하기 때문에 쿼리가 1개만 발샘
	- **단점** : FK등에 의해 순서가 맞지 않으면 에러 발생
	- 