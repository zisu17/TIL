# Mac OS에서 AWS EC2 접속하기
```bash
sudo chmod 400 njkwon-fc-seoul.pem
ssh -i "class-zisu17-seoul.pem" ec2-user@**********.ap-northeast-1.compute.amazonaws.com
```

# EC2에서 웹서비스 구동하기
웹서비스 실행하기
```bash
sudo yum update -y 
sudo yum install httpd -y 
sudo service httpd start
sudo su - 
echo  "<html><h1> <br> <br> <center> AWS를 활용한 데이터파이프라인 구축 </center> </h1></html>" > /var/www/html/index.html
```

# EC2 인스턴스 생성 시 웹서비스 자동 실행하기
아래 스크립트는 EC2 인스턴스 생성 시 사용 가능합니다.
```bash
#!/bin/bash 
yum update -y 
yum install httpd -y 
sudo service httpd start
echo  "<html><h1> <br> <br> <center> AWS를 활용한 데이터파이프라인 구축 </center> </h1></html>" > /var/www/html/index.html
```

# Java 설치하기
Java가 설치되어 있지 않을 경우 아래의 명령어를 실행합니다.
```bash
java –version
sudo yum install java-11-amazon-corretto-headless
sudo yum install java-11-amazon-corretto
java –version
```

EC2 인스턴스 생성할 때 Java를 설치합니다.
```bash
stand alone으로 Kafka를 구성할 때 사용할 수 있습니다.
#!/bin/bash 
yum install -y java-1.8.0-openjdk-devel.x86_64
```

# EC2에 Kafka Server 설치하기
## Kafka 설치하기
Kafka 다운로드 및 압축 해제하기
```bash
wget https://dlcdn.apache.org/kafka/3.0.0/kafka_2.13-3.0.0.tgz
tar xvf kafka_2.13-3.0.0.tgz
```

Kafka 환경 시작하기
```bash
ln -s kafka_2.13-3.0.0 kafka  # 키보드에서 직접 입력합니다.
cd kafka
```

Zookeeper 서버 시작하기
```bash
./bin/zookeeper-server-start.sh config/zookeeper.properties &
```

Kafka broker 시작하기
```bash
./bin/kafka-server-start.sh config/server.properties &
```

데몬 확인하기
```bash
sudo netstat -anp | egrep "9092|2181"
```

## Kafka Topic 생성하기
twitter Topic 생성
```bash
bin/kafka-topics.sh --create --topic twitter --partitions 1 --replication-factor 1 --bootstrap-server localhost:9092  &
```

Topic 확인하기
```
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

## Consumer 실행하기
```bash
./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic twitter --from-beginning
```

# EC2에 Kafka Producer 설치하기
## Kafka client 설치하기
Java 설치하기
```bash
sudo yum install -y java-1.8.0-openjdk-devel.x86_64
java –version
```

Kafka 설치하기
```bash
wget https://dlcdn.apache.org/kafka/3.0.0/kafka_2.13-3.0.0.tgz
tar xvf kafka_2.13-3.0.0.tgz
```

## Kafka producer 시작하기
여기서 IP 주소는 Kafka 서버의 private IP입니다.
```bash
bin/kafka-console-producer.sh --topic twitter --bootstrap-server 172.31.15.105:9092
```
