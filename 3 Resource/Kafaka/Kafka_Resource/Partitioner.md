
- producer가 데이터를 보내면 파티셔너를 통해 데이터가 전송됨
- 어느 파티션에 데이터를 넣을지 지정
- default는 UniformStickyPartitioner
	- 메시지 키를 가지고 있을 경우 : hash 값에 의해 어느 파티션에 들어갈지 정해짐
		- 동일한 메시지 키를 가지고 있는 레코드는 항상 동일한 파티션에 들어감
		- 순서를 지켜 메시지가 들어감
	- 메시지 키를 가지고 있지 않은 경우 : 라운드 로빈 방식으로 메시지가 파티션에 들어감
		- 배치로 모을 수 있는 최대한의 레코드를 모아서 파티션에 보냄 
- costom Partitioner : 메시지 키, 메시지 값, 또는 토픽 이름에 따라서 어느 파티션에 데이터를 보낼지 지정 가능
	- VIP고객을 위해서 VIP고객에만 더 많은 파티션을 할당 할 수 있음

※ 특정 브로커에 파티션이 몰리는 경우 **kafka-reassign-partitions.sh** 명령으로 재분배 가능