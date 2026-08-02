```shell
kafka-console-producer \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092 \
  --command-property acks=all
```
```shell
kafka-console-producer \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092 \
  --command-property acks=1
```
```shell
kafka-console-producer \
  --topic products.prices.changelog \
  --bootstrap-server localhost:9092 \
  --command-property acks=0
```
```shell
kafka-topics \
  --create \
  --topic products.prices.changelog.min-isr-2 \
  --replication-factor 3 \
  --partitions 3 \
  --config min.insync.replicas=2 \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics \
  --describe \
  --topic products.prices.changelog.min-isr-2 \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-consumer \
  --topic products.prices.changelog.min-isr-2 \
  --from-beginning \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-producer \
  --topic products.prices.changelog.min-isr-2 \
  --bootstrap-server localhost:9092 \
  --command-property aks=0
```
```
>cola 0
```
```shell
kafka-console-producer \
  --topic products.prices.changelog.min-isr-2 \
  --bootstrap-server localhost:9092 \
  --command-property aks=1
```
```
>cola 1
```
```shell
kafka-console-producer \
  --topic products.prices.changelog.min-isr-2 \
  --bootstrap-server localhost:9092 \
  --command-property aks=all
```
```
>cola all
```


# Appendix

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
kafka-server-stop --node-id=1
```
```shell
kafka-server-stop --node-id=2
```
```shell
kafka-server-stop --node-id=3
```
