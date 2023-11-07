
- ProducerRecord : 프로듀서에서 생성하는 레코드(offset은 미포함)
- send() : 레코드 전송 요청 메서드
- Partitioner : 어느 파티션으로 전송할지 지정하는 파티셔너(기본값: UniformStickyPartitioner)
- Accumulator : 배치로 묶어 전송할 데이터를 모으는 버퍼
![[Pasted image 20231101230228.png]]
![[Pasted image 20231101230128.png]]

- 기본 파티셔너
	- UniformStickyPartitioner
		- 메시지 키가 있을 경우 : 키의 해쉬값과 파티션을 매칭하여 레코드 전송
		- 메시지 키가 없을 경우 : Accumulator에서 레코드가 배치로 묶이길 기다린후 전송
		(배치로 묶이지만 파티션을 순회하며 전송하여 파티션에 분배하여 전송)
		 (RoundRobinPartitioner에 비해 향상된 성능)
	- RoundRobinPartitioner
		- 메시지 키가 있을 경우 : 키의 해쉬값과 파티션을 매칭하여 레코드 전송
		- 메시지 키가 없을 경우 : 레코드가 들어오는데로 파티션을 순회하며 전송
	                   (Accumulator에서 묶이는 정도가 적어 전송 성능 낮음)

- 커스텀 파티셔너
	- 사용자 지정 파티셔너를 위해 Partitioner 인터페이스를 제공 
	- Partitioner 인터페이스를 상속받은 사용자 정의 클래스에서 메시지 키,값에 따른 파티션 지정 로직 생성 가능


### producer 주요 옵션(필수 옵션)
- bootstrap.server
	- producer가 데이터를 전송할 대상
	- 카프카 클러스터에 속한 브로커의 **호스트 이름:포트**를 1개 이상 작성
	- 2개 이상의 정보를 넣어 일부 브로커의 장애가 발생해도 접속 가능하도록 설정 가능  
- key.serializer
	- record의 **메시지 키를 직렬화**하는 클래스를 지정
- value.serializer
	- record의 **메시지 값을 직렬화**하는 클래스를 지정

✲ string으로 serializer하지 않을 경우 장점
- int등의 값을 int 그대로 serializer하여 메모리등의 사용량을 줄일 수 있음

✲ string으로 serializer하지 않을 경우 단점
- kafka-console-consumer에서 확인이 안될 수 있음
- 디버깅이 용이 하지 않을 수 있음
- consumer에서 역직렬화에 이슈가 발생 될 수 있음

### producer 주요 옵션(선택 옵션)
- acks 
	- producer가 전송한 데이터가 브로커들에 정상적으로 저장 성공 했는지 확인하는 옵션
	- 0 : 데이터가 저장됬는지 응답이 오지 않음
	- 1 : 데이터가 저장되면 ack를 보냄 (기본값)
	- -1(all) : 데이터 저장 및 ISR(In-Sync-Replicas) 시 ack를 보냄
- linger.ms
	- 배치를 전송하기 전까지 기다리는 최소 시간 (기본값:0) (0.001초 단위)
- retries
	- 브로커로부터 에러를 받고 난 뒤 재전송을 시도하는 수 (기본값 : 2147483647)
- max.in.flight.requests.per.connection
	- 한번에 요청하는 최대 커넥션 수(기본값 : 5)
	- 설정된 값만큼 동시에 전달을 수행
- partitioner.class
	- record를 파티션에 전송할 때 적용하는 파티셔너 클래스를 지정
          (기본값 : org.apache.kafka.clients.producer.internals.DefaultPartitioner)
- enable.idempotence 
	- 멱등성 프로듀서로 동작할 지 여부를 설정 
	- version 2.5.0 (기본값 : false), version 3.0.0 (기본값 : true)
- transactional.id
	- producer가 record를 전송할 때 레코드를 트랜젝션 단위로 묶을지 여부 설정
          (기본값 : null)


## 멱등성 producer
- 데이터의 중복 적재를 막는 프로듀서
- enable.idempotence true(acks=all)로 설정가능
- 2.5.0은 default가 false이나 3.0.0부터 default가 true
- 해당 옵션 사용시 acks가 all이 되므로 속도 저하 고려하여 사용 

### 멱등성 producer의 동작
- 기본 프로듀서와 달리 프로듀서 고유 ID(PID)와 시퀀스 넘버(SID)를 함께 전달
- PID와 SID를 확인하여 중복 적재를 막음

### 멱등성 producer의 한계
- 프로듀서가 장애가 나면 PID가 바뀌어 중복 적재가 될수도 있음 