
![[Pasted image 20231107145856.png]]
- 동일한 컨슈머 그룹에 속한 컨슈머는 동일한 로직으로 동작 (예외 사항 존재)


## consumer 주요 옵션
#### 필수 옵션
- bootstrap.servers 
	- 프로듀서가 데이터를 전송할 대상
	- 카프카 클러스터에 속한 브로커의 호스트 이름:포트를 1개 이상 작성
	- 2개 이상의 브로커를 작성하여 하나의 브로커가 죽어도 동작하도록 설정 가능
- key.deserializer : 레코드의 메시지 키를 역직렬화하는 클래스를 지정
- value.deserializer : 레코드의 메시지 값을 역직렬화하는 클래스를 지정

#### 선택 옵션
- group.id
	- 컨슈머 그룹 아이디를 지정 
	- subscribe()로 토픽을 구독하여 사용할 떄는 필수로 지정 (default : null)
- auto.offset.reset
	- commit된 offset이 없을 경우 어느 offset부터 읽을지 지정(default : latest)
	- latest : 가장 최근에 넣은 offset 부터 읽음
	- earliest : 가장 오래된 offset 부터 읽음
	- none : 컨슈머 그룹이 커밋한 기록을 확인후 커밋이 없으면 오류, 있으면 이후 offset부터 반환
- enable.auto.commit : 자동/수동 커밋 설정 (default : true)
- auto.commit.interval.ms : 자동 커밋 interval 설정 (default : 5000(5초))
- max.poll.records : poll()를 통해 최대로 반환 가능한 레코드의 수 (default : 500)
- session.timeout.ms : 컨슈머와 브로커간의 timeout 시간 (default : 10000(10초))
- hearbeat.interval.ms : 하트비트 전송 간격 (default : 3000(3초))
- max.poll.interval.ms : poll()을 호출하는 간격의 최대 시간 (default : 300000(5분))
- isolation.level : 트랜젝션 프로듀서가 레코드를 트랜젝션 단위로 보낼 경우 사용 