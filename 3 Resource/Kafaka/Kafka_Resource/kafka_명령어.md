
1. 주키퍼 실행(bin/zookeeper-server-start.sh config/zookeeper.properties)
2. 브로커 실행(bin/kafka-server-start.sh config/server.properties)

### kafka 정상 실행 확인
- bin/kafka-broker-api-versions.sh --bootstrap-server localhost:9092

### kafka 브로커 topic 확인
- bin/kafka-topics.sh --bootstrap-server localhost:9092 --list


## kafka-topics.sh
- kafka 클러스터명과 topic명으로 topic 생성 가능 
- 파티션 개수, 복제 개수 등과 같은 옵션은 설정 하지 않으면 브로커의 디폴트 값을 따름
#### topic 생성
```linux
//기본 topic 생성
bin/kafka-topics.sh --create \
--bootstrap-server my-kafka:9092 \
--topic hello.kafka

//옵션을 활용한 topic 생성
bin/kafka-topics.sh --create \
--bootstrap-server my-kafka:9092 \
--partition10 \
--replication-factor 1 \
--topic hello.kafka2 \
--config retention.me=172800000

//kafka topic 확인
bin/kafka-topics.sh --bootstrap-server my-kafka:9092 --describe
```
![[Pasted image 20231031155922.png]]

#### alter
- partition 수를 늘리는 옵션
- **partition을 줄일 수 는 없음** (작은 partition을 새로 생성 권고)
```linux
//partition을 4개로 변경
bin/kafka-topics.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka --alter --partition 4
```


## kafka-configs.sh
- min.insync.replicas : 프로듀서가 데이터를 보내거나 컨슈머가 데이터를 읽을때 워터 마크 용도로 사용되고, 안전한 데이터 전송을 위해 사용됨
- `--alter`, `--add-config` 옵션을 사용하여 min.insync.replicas 옵션을 topic별 설정 가능
```linux
bin/kafka-configs.sh --bootstrap-server my-kafka:9092 \
--alter --add-config min.insync.replicas=2 \
--topic test 
```

- `--broker`, `--all`, `--describe` 옵션을 사용하여 브로커 설정 확인 가능 
```linux
bin/kafka-configs.sh --bootstrap-server my-kafka:9092 \
--broker 0 --all --describe
```


## kafka-console-producer.sh
- 토픽에 데이터를 넣는 명령어
```linux
//단순 value 값만 가진 메세지 넣기 (key = null)
bin/kafka-console-producer.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka

//메세지 key 설정
bin/kafka-console-producer.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka \
--property "parse.key=true" \
--property "key.separator=:"

//메세지키:메세지값
>key1:no1
>key2:no2
>key3:no3
```



## kafka-console-consumer.sh
- topic에 있는 데이터를 조회하기 위한 명령어
- `--from-beginning` 옵션을 통해 데이터를 처음부터 순서대로 가져올 수 있음
- `--property` 명령어로 key 를 포함한 값 확인 가능
- `--max-messages` 옵션을 사용하여 최대 컨슘 메세지 수 지정 가능
- `--partition` 옵션을 사용하여 특정 partition만 컨슘 가능
- `--group` 옵션을 사용하여 컨슈머 그룹을 기반으로 확인 가능 
  (컨슈머 그룹으로 토픽의 레코드를 가져갈 경우 어느 레코드까지 읽었는지에 대한 데이터를 브로커에  커밋) (**`__consumer_offsets` topic**에 커밋함)
```linux
bin/kafka-console-consumer.sh \
--bootstrap-server my-kafka:9092 \
--topic hello.kafka --from-beginning

//메세지 키를 확인하는 명령어
bin/kafka-console-consumer.sh \
--bootstrap-server my-kafka:9092 \
--topic hello.kafka \
--property print.key=true \
--property key.separator=":" \
--from-beginning

//최대 컨슘 수 지정
bin/kafka-console-consumer.sh \
--bootstrap-server my-kafka:9092 \
--topic hello.kafka --from-beginning --max-messages 1

//특정 partition 지정 컨슘
bin/kafka-console-consumer.sh \
--bootstrap-server my-kafka:9092 \
--topic hello.kafka --partition 2 --from-beginning

//group 지정 컨슘
bin/kafka-console-consumer.sh \
--bootstrap-server my-kafka:9092 \
--topic hello.kafka \
--group hello-group --from-beginning
```