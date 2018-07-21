<h1>Kafka Quick Start</h1>

<h2>1. docker 및 docker-compose 설치<h2>
<pre>
# apt-get install docker.io
# apt-get install docker-compose
</pre>

<h2>2. kafka container pull & running</h2>

<h3>2.1 git 설치</h3>
<pre>
# apt-get install git
</pre>

<h3>2.2 kafka container를 정의하는 프로젝트 다운로드 및 yml파일 수정</h3>
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

<h3>2.3 docker-compose 시작</h3>
<pre>
# docker-compose -f docker-compose-single-broker.yml up # Single Broker
</pre>

<h2>3. KAFKA Topic 생성, 전송, 수신</h2>

<h3>3.1 Kafka 소스 다운로드 및 압축해제</h3>
<pre>
# wget wget http://apache.tt.co.kr/kafka/1.1.0/kafka_2.11-1.1.0.tgz
# tar -xzf kafka_2.11-1.1.0.tgz

# cd kafka_2.11-1.1.0/
# apt install default-jre  <== kafka shell을 실행하기 위해 필요
</pre>

<h3>3.2 Topic 생성</h3>
<pre>
# bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test123

생성된 Topic 확인
# bin/kafka-topics.sh --list --zookeeper localhost:2181
test		<== container실행시 사용한 Topic
test123		<== 좀 전에 생성한 Topic
</pre>

<h3>3.3 Topic 전송</h3>
<pre>
# bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test123
This is a message
This is another message
</pre>

<h3>3.4 Topic 수신</h3>
</pre>
# bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test123 --from-beginning
This is a message
This is another message
</pre>
