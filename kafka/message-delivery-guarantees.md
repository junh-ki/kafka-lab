```shell
kafka-console-producer \
  --topic products.prices.changelog.min-isr-2 \
  --bootstrap-server localhost:9092 \
  --command-property acks=all \
  --command-property enable.idempotence=true
```
