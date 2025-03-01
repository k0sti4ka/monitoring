# monitoring

A monitoring solution for Docker hosts and containers with [Prometheus](https://prometheus.io/), [Grafana](http://grafana.org/), [cAdvisor](https://github.com/google/cadvisor),
[NodeExporter](https://github.com/prometheus/node_exporter) and alerting with [AlertManager](https://github.com/prometheus/alertmanager).

# Install on main host

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


# Install on remote/tracking hosts 

## Install cadvisor on tracking host


```bash
docker run -d \
  --name cadvisor \
  --restart unless-stopped \
  --privileged \
  -p 9091:8080 \
  -v /:/rootfs:ro \
  -v /var/run:/var/run:ro \
  -v /sys:/sys:ro \
  -v /var/lib/docker/:/var/lib/docker:ro \
  -v /cgroup:/cgroup:ro \
  -v /dev/kmsg:/dev/kmsg \
  -l org.label-schema.group="monitoring" \
  gcr.io/cadvisor/cadvisor:v0.51.0 \
  --enable_metrics=cpu,memory,network \
  --store_container_labels=false \
  --docker_only=true \
  --housekeeping_interval=10s
  ```
## Install nodeexporter  on tracking host
```bash
docker run -d \
  --name nodeexporter \
  --restart unless-stopped \
  --network host \
  -v /proc:/host/proc:ro \
  -v /sys:/host/sys:ro \
  -v /:/rootfs:ro \
  -l org.label-schema.group="monitoring" \
  prom/node-exporter:v1.9.0 \
  --path.procfs=/host/proc \
  --path.rootfs=/rootfs \
  --path.sysfs=/host/sys \
  --collector.filesystem.mount-points-exclude='^/(sys|proc|dev|host|etc)($$|/)'
  ```