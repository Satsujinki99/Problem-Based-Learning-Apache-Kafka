# Problem-Based-Learning-Apache-Kafka

## Installing Kafka Zookeeper

1. nano docker-compose.yml

```

version: '3'

services:
  zookeeper:
    image: confluentinc/cp-zookeeper:7.3.0
    container_name: zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - "2181:2181"

  kafka:
    image: confluentinc/cp-kafka:7.3.0
    container_name: kafka
    depends_on:
      - zookeeper
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"

```

2. docker-compose up -d

<img width="411" alt="Image" src="https://github.com/user-attachments/assets/e9802915-3b7f-4c98-9ed3-9cb453538185" />

3. docker ps

<img width="1295" alt="Image" src="https://github.com/user-attachments/assets/01e44777-11b3-400b-b115-0501c1b7d44b" />




## Buat topik suhu dan kelembaban

1. Topik Suhu
``` 
docker exec -it 5529a5bcf360 kafka-topics.sh --create --topic sensor-suhu-gudang \
    --bootstrap-server localhost:9092 \
    --replication-factor 1 \
    --partitions 3
```
2. Topik Kelembaban

```
docker exec -it 5529a5bcf360 kafka-topics.sh --create --topic sensor-kelembaban-gudang \
    --bootstrap-server localhost:9092 \
    --replication-factor 1 \
    --partitions 3
```

## Buat file python untuk create producer data, pyspark consumer

1. Install Lib
```
pip install kafka-python
pip install pyspark findspark
```

2. Buat File
```
nano producer_suhu.py
nano producer_kelembaban.py
nano pyspark_consumer.py
```

## Run file
```
python3 producer_suhu.py
python3 producer_kelembaban.py
python3 pyspark_consumer.py
```

# Output
<img width="557" alt="Image" src="https://github.com/user-attachments/assets/987aed1b-b143-4212-832a-3a698c015a2c" />

<img width="639" alt="Image" src="https://github.com/user-attachments/assets/2aad0484-b21f-49e2-a739-d0a4ec36522c" />

<img width="314" alt="Image" src="https://github.com/user-attachments/assets/cd0fb4d4-9b3e-491a-b8b5-7d6f44c77bfd" />

<img width="314" alt="Image" src="https://github.com/user-attachments/assets/3e2e3dfb-38c7-4812-95a1-e3590529b311" />

<img width="496" alt="Image" src="https://github.com/user-attachments/assets/1be73629-0f7f-4151-9eaf-4c2b8f8cc818" />
