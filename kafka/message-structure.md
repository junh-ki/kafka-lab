```shell
kafka-topics \
  --create \
  --topic products.prices.changelog.keys \
  --replication-factor 2 \
  --partitions 2 \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-producer \
  --topic products.prices.changelog.keys \
  --formatter-property parse.key=true \
  --formatter-property key.separator=: \
  --bootstrap-server localhost:9092
```
```
# Producer window:
> coffee pads:10
> cola:2
> coffee pads:11
> coffee pads:12
```
```shell
kafka-console-consumer \
  --from-beginning \
  --topic products.prices.changelog.keys \
  --bootstrap-server localhost:9092
```
```shell
kafka-console-consumer \
  --from-beginning \
  --topic products.prices.changelog.keys \
  --formatter-property print.key=true \
  --formatter-property key.separator=":" \
  --bootstrap-server localhost:9092
```
