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
```shell
kafka-topics --create \
  --topic products.prices.changelog.replication-3 \
  --replication-factor 3 \
  --partitions 3 \
  --config min.insync.replicas=2 \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics --describe \
  --topic products.prices.changelog.replication-3 \
  --bootstrap-server localhost:9092
```
```shell
kafka-server-stop --node-id=3
```
```shell
kafka-topics --describe \
  --topic products.prices.changelog.replication-3 \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics --describe \
  --bootstrap-server localhost:9092 \
  --under-replicated-partitions
``` 
```shell
kafka-topics --describe \
  --bootstrap-server localhost:9092 \
  --under-min-isr-partitions
``` 
```shell
kafka-server-start -daemon /opt/homebrew/opt/kafka/config/kafka3.properties
```
```shell
kafka-topics --describe \
  --topic products.prices.changelog.replication-3 \
  --bootstrap-server localhost:9092
```
```shell
kafka-leader-election \
  --election-type=preferred \
  --all-topic-partitions \
  --bootstrap-server localhost:9092
```
