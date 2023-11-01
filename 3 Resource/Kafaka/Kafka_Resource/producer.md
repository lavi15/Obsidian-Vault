
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