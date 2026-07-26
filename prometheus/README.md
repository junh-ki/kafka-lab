## Installing the JMX Exporter (for Kafka metrics on 9101-9103)

There's no Homebrew formula for it — download the Java agent jar directly:

```shell
curl -L -o /opt/homebrew/opt/kafka/jmx_prometheus_javaagent.jar \
  https://github.com/prometheus/jmx_exporter/releases/download/v1.6.0/jmx_prometheus_javaagent-1.6.0.jar
```

Download the Kafka KRaft exporter config to `/opt/homebrew/opt/kafka/config/jmx-exporter.yml` (maps the actual Kafka JMX beans — broker, topic, partition metrics — rather than a bare passthrough):

```shell
curl -L -o /opt/homebrew/opt/kafka/config/jmx-exporter.yml \
  https://raw.githubusercontent.com/prometheus/jmx_exporter/refs/heads/main/examples/kafka-kraft-3_0_0.yml
```

Attach the agent when starting each broker (port must match `prometheus.yml`'s `kafka` job targets: 9101/9102/9103 for kafka1/kafka2/kafka3):

```shell
KAFKA_OPTS="-javaagent:/opt/homebrew/opt/kafka/jmx_prometheus_javaagent.jar=9101:/opt/homebrew/opt/kafka/config/jmx-exporter.yml" \
  kafka-server-start -daemon /opt/homebrew/opt/kafka/config/kafka1.properties
```
```shell
KAFKA_OPTS="-javaagent:/opt/homebrew/opt/kafka/jmx_prometheus_javaagent.jar=9102:/opt/homebrew/opt/kafka/config/jmx-exporter.yml" \
  kafka-server-start -daemon /opt/homebrew/opt/kafka/config/kafka2.properties
```
```shell
KAFKA_OPTS="-javaagent:/opt/homebrew/opt/kafka/jmx_prometheus_javaagent.jar=9103:/opt/homebrew/opt/kafka/config/jmx-exporter.yml" \
  kafka-server-start -daemon /opt/homebrew/opt/kafka/config/kafka3.properties
```
```shell
curl localhost:9101
curl localhost:9102
curl localhost:9103
```

## Running Prometheus

Prometheus is installed via `brew install prometheus`, but run it directly (not via `brew services`) so it uses this directory's config instead of Homebrew's default:

```shell
cd prometheus
nohup prometheus --config.file=prometheus.yml > prometheus.log 2>&1 &
```

- Web UI: http://localhost:9090
- `rules.yml` is referenced by relative path in `prometheus.yml`, so the command must be run from inside this directory.
- Logs go to `prometheus.log`; the PID is printed by `&` (or find it with `pgrep -f 'prometheus --config.file=prometheus.yml'`).
- Stop it with `kill <PID>`.

## Stopping Prometheus

```shell
pkill -f 'prometheus --config.file=prometheus.yml'
```

Validate config/rules after editing:

```shell
promtool check config prometheus.yml
promtool check rules rules.yml
```
