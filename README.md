# Configuring Kafka

```shell
mkdir /opt/homebrew/opt/kafka/config
touch /opt/homebrew/opt/kafka/config/kafka1.properties
touch /opt/homebrew/opt/kafka/config/kafka2.properties
touch /opt/homebrew/opt/kafka/config/kafka3.properties

open /opt/homebrew/opt/kafka/config/kafka1.properties
open /opt/homebrew/opt/kafka/config/kafka2.properties
open /opt/homebrew/opt/kafka/config/kafka3.properties
```
```kafka1.properties
node.id=1
broker.id=1
log.dirs=/opt/homebrew/opt/kafka/data/kafka1
listeners=PLAINTEXT://:9092,CONTROLLER://:9192
advertised.listeners=PLAINTEXT://localhost:9092
process.roles=broker,controller
controller.quorum.voters=1@localhost:9192,2@localhost:9193,3@localhost:9194
controller.listener.names=CONTROLLER
listener.security.protocol.map=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT
```
```kafka2.properties
node.id=2
broker.id=2
log.dirs=/opt/homebrew/opt/kafka/data/kafka2
listeners=PLAINTEXT://:9093,CONTROLLER://:9193
advertised.listeners=PLAINTEXT://localhost:9093
process.roles=broker,controller
controller.quorum.voters=1@localhost:9192,2@localhost:9193,3@localhost:9194
controller.listener.names=CONTROLLER
listener.security.protocol.map=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT
```
```kafka3.properties
node.id=3
broker.id=3
log.dirs=/opt/homebrew/opt/kafka/data/kafka3
listeners=PLAINTEXT://:9094,CONTROLLER://:9194
advertised.listeners=PLAINTEXT://localhost:9094
process.roles=broker,controller
controller.quorum.voters=1@localhost:9192,2@localhost:9193,3@localhost:9194
controller.listener.names=CONTROLLER
listener.security.protocol.map=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT
```

# Preparing the data directories

```shell
mkdir -p /opt/homebrew/opt/kafka/data/kafka1 \
  /opt/homebrew/opt/kafka/data/kafka2 \
  /opt/homebrew/opt/kafka/data/kafka3
```
```shell
open .zshrc
```
```
export KAFKA_CLUSTER_ID=3jT9yIIYS-CzIvkk69WhdA
```
-> from `kafka-storage random-uuid`
```shell
kafka-storage format -t $KAFKA_CLUSTER_ID \
  -c /opt/homebrew/opt/kafka/config/kafka1.properties
```
```shell
kafka-storage format -t $KAFKA_CLUSTER_ID \
  -c /opt/homebrew/opt/kafka/config/kafka2.properties
```
```shell
kafka-storage format -t $KAFKA_CLUSTER_ID \
  -c /opt/homebrew/opt/kafka/config/kafka3.properties
```

# Starting Kafka (daemon mode)

```shell
kafka-server-start -daemon /opt/homebrew/opt/kafka/config/kafka1.properties
```
```shell
kafka-server-start -daemon /opt/homebrew/opt/kafka/config/kafka2.properties
```
```shell
kafka-server-start -daemon /opt/homebrew/opt/kafka/config/kafka3.properties
```
```shell
kafka-broker-api-versions --bootstrap-server localhost:9092,localhost:9093,localhost:9094
```

# Stopping Kafka

## Stop all Kafka brokers

```shell
kafka-server-stop
```

## Stop a single Kafka broker

```shell
kafka-server-stop --node-id=1
```

## Stop Kafka brokers with a specific role

```shell
kafka-server-stop --process-role=broker
```

# Producing messages

```shell
kafka-topics \
  --create \
  --topic products.prices.changelog \
  --partitions 1 \
  --replication-factor 1 \
  --bootstrap-server localhost:9092
```
```shell
echo "coffee pads 10" | kafka-console-producer \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-producer \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092
```

# Consuming messages

```shell
kafka-console-consumer \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-consumer \
  --topic products.prices.changelog \
  --from-beginning \
  --bootstrap-server localhost:9092
```

# Viewing topics

```shell
kafka-topics \
  --describe \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics \
  --list \
  --bootstrap-server localhost:9092
```

# Create, customize, and delete topics

```shell
kafka-topics \
  --delete \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics \
  --create \
  --topic products.prices.changelog \
  --replication-factor 2 \
  --partitions 2 \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics \
  --describe \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics \
  --alter \
  --topic products.prices.changelog \
  --partitions 3 \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics \
  --describe \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092
```
