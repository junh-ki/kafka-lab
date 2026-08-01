```shell
kafka-topics \
  --create \
  --topic products.prices.replication \
  --partitions 3 \
  --replication-factor 3 \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics \
  --describe \
  --topic products.prices.replication \
  --bootstrap-server localhost:9092
```
