## Running Prometheus

Prometheus is installed via `brew install prometheus`, but run it directly (not via `brew services`) so it uses this directory's config instead of Homebrew's default:

```bash
cd prometheus
nohup prometheus --config.file=prometheus.yml > prometheus.log 2>&1 &
```

- Web UI: http://localhost:9090
- `rules.yml` is referenced by relative path in `prometheus.yml`, so the command must be run from inside this directory.
- Logs go to `prometheus.log`; the PID is printed by `&` (or find it with `pgrep -f 'prometheus --config.file=prometheus.yml'`).
- Stop it with `kill <PID>`.

## Stopping Prometheus

```bash
pkill -f 'prometheus --config.file=prometheus.yml'
```

Validate config/rules after editing:

```bash
promtool check config prometheus.yml
promtool check rules rules.yml
```
