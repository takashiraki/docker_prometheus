# docker_prometheus

A minimal Docker-based [Prometheus](https://prometheus.io/) monitoring setup.

## Overview

Runs Prometheus in a Docker container, integrated with an external reverse proxy network. Prometheus scrapes its own metrics as a baseline setup.

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

| Method | URL |
|--------|-----|
| Direct | http://localhost:9090 |
| Via proxy | http://prometheus.localhost |

## Configuration

### docker-compose.yml

| Setting | Value |
|---------|-------|
| Image | `prom/prometheus:latest` |
| Container name | `my_prometheus` |
| Port | `9090` |
| Restart policy | `unless-stopped` |
| Network | `my_proxy_network` (external) |

### prometheus.yml

| Setting | Value |
|---------|-------|
| Global scrape interval | 15s |
| Self-scrape interval | 5s |
| Self-scrape target | `my_prometheus:9090` |

## Branches

| Branch | Description |
|--------|-------------|
| `master` | Base Prometheus setup |
| `node-exporter` | Adds Node Exporter for host metrics |
