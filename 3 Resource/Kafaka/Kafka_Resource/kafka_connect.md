
![[Pasted image 20231107222308.png]]
- 데이터 파이프라인 생성 시 반복 작업을 줄이고 효율적으로 전송 가능하도록 해주는 어플
- 특정한 작업 형태를 템플릿 형태로 만들어 놓은 커넥터를 실행하여 반복을 줄임 
- 커넥트에 커넥터 생성 명령을 하면 커넥터와 태스크를 생성함
- 커넥터는 테스크를 관리(**태스크는 커넥터에 종속적**)
- **8083port**로 호출 가능하며 HTTP 메서드 기반 API 제공
![[Pasted image 20231107224700.png]]

#### GUI : https://github.com/kakao/kafka-connect-web
#### image : docker pull officialkakao/kafka-connect-web


### 컨버터 
- 옵셔널한 설정으로 데이터를 처리하기 전에 스키마를 변경하도록 도와줌
- JsonConverter, StringConverter, ByteArrayConverter를 지원

### 트랜스폼
- 옵셔널한 설정으로 데이터 처리 시 각 메시지 단위로 데이터를 간단하게 변환
- JSON 데이터를 커넥터에서 사용할 때 트렌스폼을 사용하면 특정 키를 삭제 및 추가 가능
- Cast, Drop, ExtractField등이 존재

## 커넥터
### 소스 커넥터
- 파일의 데이터를 카프카 토픽으로 전송하는 프로듀서 역활

### 싱크 커넥터
- 토픽의 데이터를 카프카 토픽으로 전송하는 프로듀서 역활을 함

### 오픈소스 커넥터
- 컨플루언트 허브(https://www.confluent.io/hub)에서 다운 가능 

### 커스텀 커넥터
- task, connector를 커스텀 하여 사용

## HA
### 단일 모드 커넥트
- 1개의 프로세스만 실행 
- HA가 구성되지 않아 SPOF(Single Point Of Failure)이 될 수 있음

### 분산 모드 커넥
- 2대 이상의 서버에서 클러스터 형태로 운영함으로써 안전하게 운영 가능
- 2개 이상의 커넥트가 클러스터로 묶이면 Fail over가 되어 서비스가 죽지 않음


## 중요도 지정 기준
- 커넥터를 개발할때 옵션값의 중요도를 enum 클래스로 지정 가능
- HIGH : 반드시 사용자가 입력이 필요한 옵션
- MEDIUM : 입력값이 없어도 상관없고 default 값이 있는 옵션
- LOW : 입력값이 없어도 상관없는 옵션
