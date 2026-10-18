# Monitoring Architecture

This project monitors two independent Linux virtual machines.

```mermaid
flowchart TB
    browser["Admin Browser"]

    subgraph vm1["Monitoring VM — 192.168.0.124"]
        grafana["Grafana :3000"]
        prometheus["Prometheus :9090"]
        exporter1["Node Exporter container :9100"]

        grafana -->|"PromQL queries"| prometheus
        prometheus -->|"Scrape every 15 seconds"| exporter1
    end

    subgraph vm2["Remote Linux VM — 192.168.0.111"]
        exporter2["Node Exporter system service :9100"]
    end

    browser -->|"HTTP :3000"| grafana
    prometheus -->|"Scrape metrics over HTTP :9100"| exporter2
```

## Components

- **Grafana** displays dashboards and manages alerting.
- **Prometheus** collects and stores time-series metrics.
- **Node Exporter on VM 1** exposes metrics from the monitoring host.
- **Node Exporter on VM 2** exposes metrics from the remote Linux host.
- **Prometheus scrape interval:** 15 seconds.

The IP addresses shown are specific to the current lab environment.
Replace them with the appropriate addresses when deploying the project
in another environment.
