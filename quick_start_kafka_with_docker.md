<h1>Kafka Quick Start</h1>

1. docker 및 docker-compose 설치
<pre>
# apt-get install docker.io
# apt-get install docker-compose
</pre>

2. kafka container pull & running

2.1 git 설치
<pre>
# apt-get install git
</pre>

2.2 kafka container를 정의하는 프로젝트 다운로드 및 yml파일 수정
<pre>
# git clone https://github.com/wurstmeister/kafka-docker
# cd kafka-docker
# cat docker-compose-single-broker.yml
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"
  kafka:
    build: .
    ports:
      - "9092:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 127.0.0.1	<= 요기를 수정
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
</pre>

2.3 docker-compose 시작
<pre>
# docker-compose -f docker-compose-single-broker.yml up # Single Broker
</pre>

3. KAFKA Topic 생성, 전송, 수신

3.1 Kafka 소스 다운로드 및 압축해제
<pre>
# wget wget http://apache.tt.co.kr/kafka/1.1.0/kafka_2.11-1.1.0.tgz
# tar -xzf kafka_2.11-1.1.0.tgz

# cd kafka_2.11-1.1.0/
# apt install default-jre  <== kafka shell을 실행하기 위해 필요
</pre>

3.2 Topic 생성
<pre>
# bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test123

생성된 Topic 확인
# bin/kafka-topics.sh --list --zookeeper localhost:2181
test		<== container실행시 사용한 Topic
test123		<== 좀 전에 생성한 Topic
</pre>

3.3 Topic 전송
<pre>
# bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test123
This is a message
This is another message
</pre>

3.4 Topic 수신
</pre>
# bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test123 --from-beginning
This is a message
This is another message
</pre>
