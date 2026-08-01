# Consumer Part (open two terminals side by side for comparison)

```shell
kafka-console-consumer \
  --from-beginning \
  --topic products.prices.changelog.multi-partitions-keys \
  --formatter-property print.key=true \
  --formatter-property key.separator=":" \
  --formatter-property print.partition=true \
  --group products \
  --bootstrap-server localhost:9092
```

# Producer Part

```shell
kafka-console-producer \
  --topic products.prices.changelog.multi-partitions-keys \
  --property parse.key=true \
  --property key.separator=: \
  --bootstrap-server localhost:9092
```
```
> energy drink:2
> energy drink:3
> cola:2
> cola:5
> energy drink:1
> cola:2
```
