# monitoring

A monitoring solution for Docker hosts and containers with [Prometheus](https://prometheus.io/), [Grafana](http://grafana.org/), [cAdvisor](https://github.com/google/cadvisor),
[NodeExporter](https://github.com/prometheus/node_exporter) and alerting with [AlertManager](https://github.com/prometheus/alertmanager).

## Install

Clone this repository on your Docker host, cd into monitoring directory and run compose up:

```bash
git clone https://github.com/k0sti4ka/monitoring.git
```
### Go to monitoring/prometheus and add your hosts for job_name: 'nodeexporter' and job_name: 'cadvisor'


```bash
docker compose up -d
```

## Setup Grafana

Navigate to `http://<host-ip>:3000` and login with user ***admin*** password ***admin***.

Grafana is preconfigured with dashboards and Prometheus as the default data source:

* Name: Prometheus
* Type: Prometheus
* Url: [http://prometheus:9090](http://prometheus:9090)
* Access: proxy

