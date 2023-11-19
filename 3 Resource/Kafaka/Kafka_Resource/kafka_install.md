
https://blog.voidmainvoid.net/325
#### 주키퍼 (인스턴스 3개)
- 모든 인스턴스에 동일하게 진행
- 주키퍼 인스톨
```linux
wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.12/zookeeper-3.4.12.tar.gz
```
- 주키퍼 압축해제
```
tar xvf zookeeper-3.4.12.tar.gz
```


- 주키퍼 configuration 설정 **(/zookeeper/conf/zoo.cfg)**
```cfg
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=20
syncLimit=5
server.1=test-broker01:2888:3888
server.2=test-broker02:2888:3888
server.3=test-broker03:2888:3888
```

- 주키퍼 앙상블 생성
	- zookeeper마다 myid 파일 생성 필요(/var/lib/zookeeper/myid)
	- myid는 각자의 파일마다 숫자만 하나 넣음 (1번은 1, 2번은 2)
	- ex) cat /var/lib/zookeeper/myid     =>   1

- 주키퍼 실행
```linux
$ ./bin/zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /home/ec2-user/zookeeper-3.4.12/bin/../conf/zoo.cfg
Strating zookeeper ... STARTED
```

#### JDK 설치
```
sudo apt install openjdk-17-jdk
```

#### kafka 설치
- kafka 설치
```linux
wget https://archive.apache.org/dist/kafka/2.1.0/kafka_2.11-2.1.0.tgz
```
- kafka 압축해제
```linux
tar xvf kafka_2.11-2.1.0.tgz
```

- broker.id, zookeeper 관련 설정 , listener 설정 **(/kafka/config/server.properties)**
```
# The id of the broker. This must be set to a unique integer for each broker.
broker.id=0
listeners=PLAINTEXT://:9092
advertised.listeners=PLAINTEXT://test-broker01:9092
zookeeper.connect=test-broker01:2181,test-broker02:2181,test-broker03/test
```

- kafka 실행
```linux
./bin/kafka-server-start.sh ./config/server.properties
```