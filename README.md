# docker_prometheus

A Docker Compose-based monitoring stack combining metrics collection, visualization, and distributed tracing.

## Services

| Service | Image | Role |
|---------|-------|------|
| `prometheus` | `prom/prometheus:latest` | Metrics scraping and alert rule evaluation |
| `grafana` | `grafana/grafana:latest` | Dashboard visualization |
| `tempo` | `grafana/tempo:latest` | Distributed tracing (OTLP) |

## Requirements

- Docker & Docker Compose
- An external Docker network named `my_proxy_network`

Create the network if it doesn't exist:

```bash
docker network create my_proxy_network
```

## Usage

### Start

```bash
docker compose up -d
```

### Stop

```bash
docker compose down
```

## Access

| Service | Via Proxy | Direct |
|---------|-----------|--------|
| Prometheus | http://prometheus.localhost | — |
| Grafana | http://grafana.localhost | — |
| Tempo | http://tempo.localhost | — |

> Direct ports are not exposed. Access all services through the proxy network.

## Configuration

### prometheus/prometheus.yml

| Setting | Value |
|---------|-------|
| Global scrape interval | 15s |
| Rule evaluation interval | 15s |
| Prometheus self-scrape interval | 5s |
| Rule files | `prometheus.rules.yml` |

### prometheus/prometheus.rules.yml

| Type | Record | Expression |
|------|--------|------------|
| Recording rule | `job_instance_mode:node_cpu_seconds:avg_rate5m` | `avg by (job, instance, mode) (rate(node_cpu_seconds_total[5m]))` |

### tempo/tempo.yaml

| Setting | Value |
|---------|-------|
| HTTP port | 3200 |
| OTLP/HTTP endpoint | `0.0.0.0:4318` |
| OTLP/gRPC endpoint | `0.0.0.0:4317` |
| Storage backend | local (`/var/lib/tempo`) |

### grafana/provisioning/datasources/tempo.yaml

The Tempo datasource (`http://my-tempo:3200`) is automatically provisioned when Grafana starts.

## Volumes

| Volume | Purpose |
|--------|---------|
| `prometheus_data` | Prometheus TSDB |
| `grafana_data` | Grafana settings and dashboards |
| `tempo_data` | Tempo trace data |
