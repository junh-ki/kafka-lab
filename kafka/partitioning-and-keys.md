# Without Keys

```shell
kafka-topics \
  --create \
  --topic products.prices.changelog.multi-partitions \
  --partitions 2 \
  --replication-factor 1 \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-producer \
  --topic products.prices.changelog.multi-partitions \
  --bootstrap-server localhost:9092
```
```
> coffee pads 10
> cola 2
> energy drink 3
> coffee pads 11
> coffee pads 12
> coffee pads 10
```
```shell
kafka-console-consumer \
  --topic products.prices.changelog.multi-partitions \
  --from-beginning \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-consumer \
  --topic products.prices.changelog.multi-partitions \
  --from-beginning \
  --formatter-property print.partition=true \
  --bootstrap-server localhost:9092
```

# With Keys

```shell
kafka-topics \
  --delete \
  --topic products.prices.changelog.multi-partitions-keys \
  --bootstrap-server localhost:9092
```
```shell
kafka-topics \
  --create \
  --topic products.prices.changelog.multi-partitions-keys \
  --partitions 2 \
  --replication-factor 1 \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-producer \
  --topic products.prices.changelog.multi-partitions-keys \
  --property parse.key=true \
  --property key.separator=":" \
  --bootstrap-server localhost:9092
```
```
> coffee pads:10
> cola:2
> cola:1
> energy drink:3
> coffee pads:11
> coffee pads:12
> energy drink:4
> coffee pads:10
```
```shell
kafka-console-consumer \
  --from-beginning \
  --topic products.prices.changelog.multi-partitions-keys \
  --formatter-property print.key=true \
  --formatter-property key.separator=":" \
  --formatter-property print.partition=true \
  --bootstrap-server localhost:9092
```
