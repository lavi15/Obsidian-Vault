
![[Pasted image 20231107170918.png]]
![[Pasted image 20231107171402.png]]
- java 기반 라이브러리
- 토픽에 적재된 데이터를 실시간으로 변환하여 다른 토픽에 적재하는 라이브러리
- 태스크(task) : 스트림즈를 실행시키면 생기는 데이터 처리 최소 단위(default : 3개)
- 장애가 발생하더라도 안정적으로 운영할 수 있도록 2개 이상의 서버로 구성하여 사용

### 스트림즈를 사용해야 하는 이유
- streams는 데이터 처리에 있어 다양한 기능을 스트림DSL로 제공하며 프로세서 API를 활용하여 기능 확장 가능 

## 프로세서와 스트림
![[Pasted image 20231107171736.png]]
- 프로세서 : 토폴로지를 이루는 하나의 노드
- 스트림 : 노드와 노드를 이은 선 (컨슈머와 프로듀서에서의 레코드와 동일)
- 소스 프로세서 : 토픽을 가져오는 프로세서
- 스트림 프로세서 : 데이터를 변환하는 프로세서
- 싱크 프로세서 : 데이터를 처리하는 프로세서

## KStream, KTable, GlobalKTable
### KStream
- 메시지 키와 값으로 이루어져 있음
- KStream으로 조회시 토픽에 존재하는 모든 레코드가 출력됨
- KStream은 컨슈머로 토픽을 구독하는 것과 동일한 선상에서 사용하는 것 

### KTable
- 메시지 키를 기준으로 묶어서 사용
- 유니크한 메시지 키를 기준으로 가장 최신 레코드를 사용 
- topic의 데이터를 Map과 같이 key, value 데이터로 사용 가능

**✲코파티셔닝(co-partitioning)**
- 조인을 하는 2개의 토픽을 파티션 수를 동일하게 하고 파티셔닝 전략을 동일하게 하는 작업
- KStream,KTable은 코파티셔닝이 되지 않으면 조인시 에러 발생

### GlobalKTable
- 코파티셔닝이 되어 있지 않은 경우 조인을 사용할 때 사용
- GlobalKTable을 태스크가 다 가지고 있으며 해당 값으로 조인을 사용
- GlobalKTable의 값을 태스크가 다 가지고 있기 떄문에 GlobalKTable의 크기가 클경우 부하 발생
- GlobalKTable의 데이터 양이 적을 경우에만 사용하는걸 권고


## 스트림즈DSL 주요 옵션
#### 필수 옵션
- bootstrap.servers : 프로듀서가 데이터를 전송할 대상
- application.id
	- 스트림즈 어플리케이션을 구분하기 위한 고유의 ID
	- application.id를 기반으로 컨슈머 그룹이 운용됨
	- 컨슈머 그룹 리셋시 application.id으로 컨슈머 그룹으로 지정후 리셋

#### 선택 옵션
- default.key.serde : 레코드의 메시지 키를 직렬화, 역직렬화 하는 클래스를 지정
- default.value.serde : 레코드의 메시지 값을 직렬화, 역직렬화 하는 클래스를 지정
- num.stream.threads : 스트림 프로세싱 실행 시 실행될 스레드 수를 지정 (default : 1)
- state.dir : 상태기반 데이터 처리를 할 때 데이터를 저장할 디렉토리를 지정
  (default : /tmp/kafka-streams)
