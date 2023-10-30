
## Broker
- kafka broker는 카프카가 설치되어 있는 서버의 단위 3개 이상을 권고
- partition(broker의 group)이 1일때(partition 1개당 broker 개수 설정 가능) replication이 1이면 한개의 broker에만 데이터가 쌓이고, partition이 1이고 replication이 2일 경우 1개의 브로커에 데이터가 쌓이고 1개의 브로커에 복사본이 쌓임 
- 원본 데이터가 leader partition, 복사본 데이터가 follower partition이라고 하고 이를 합쳐서 ISR(In Sync Replica)이라고 함
![[Pasted image 20231029211848.png]]

### ack
- producer에는 ack라는 고가용성을 위한 상세 옵션이 존재
- ack는 0, 1, all 설정 가능
	- 0 : request를 보낸 후 정상적으로 leader partition이 받았는지 알 수 없으나 빠름
	- 1 : leader partition이 데이터를 받은 후 응답 패킷을 보냄, 다만 나머지 follower partition에 복제가 정상적으로 되었는지 알 수 없음
	- all : 1 옵션에 추가로 follower partition에 복제가 되었는지도 추가 응답을 받음 다만 확인 하는 부분이 많아 속도가 현저히 느림
- 3개 이상의 broker를 사용할 경우 replication은 3개를 권고(많으면 리소스 사용량이 늘어남)

### 주키퍼
- 분산 코디네이션 서비스를 제공하는 오픈 소스
- 분산 코디네이션 서비스 : 분산 시스템에서 시스템 간의 정보 공유, 상태 체크, 서버들 간의 동기화를 위한 락 등을 처리해 주는 서비스 