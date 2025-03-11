---
{"dg-publish":true,"tags":["docker","monitor","container"],"permalink":"/developer/Home Lab/Prometheus/","dgPassFrontmatter":true}
---

Creating a [[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]] is like gardening. The more it grows, the harder it is to manage. Currently I monitor my [[developer/Hardware/Intel NUC\|Server]] with a simple [[developer/Home Lab/Glances\|Glances]] solution. This gets me the host's `CPU` `RAM` `Memory Storage` `Network` etc. 

Now I need finer grain monitoring. With all my fancy [[developer/Home Lab/Docker\|Docker]] containers running all sorts of apps, I need to see which apps / services are really hogging the resources

> [!error] when editing the `/etc/docker/daemon.json` I totally borked my docker service. So fixing or removing that file will help the daemon run again [[developer/Docker🐳/Docker Daemon Goes Down\|Docker Daemon Goes Down]]
## Step 1: Docker Config
Get the docker IP as it will be different from the host IP

```shell
ip addr show docker0
    8: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
        link/ether 02:42:18:5c:09:7f brd ff:ff:ff:ff:ff:ff
        inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
           valid_lft forever preferred_lft forever
        inet6 fe80::42:18ff:fe5c:97f/64 scope link
           valid_lft forever preferred_lft forever
```

You may want to skip this part if you intend on using **Node Exporter** to monitor the host.
`/etc/docker/daemon.json`
```json
{
   "metrics-addr": "172.17.0.1:9323"
}
```

```shell
sudo service docker restart
```
## Step 2:  Prometheus & Node Exporter

### Config
`prometheus.yml`
```yml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: "codelab-monitor"

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'
    static_configs:
      - targets: ["localhost:9090"]
      - 
  # may skip this part if you skipped the above 1st step.
  - job_name: 'docker'
    static_configs:
      - targets: ["host.docker.internal:9323"]

  - job_name: 'node_exporter'
    static_configs:
      - targets: ["node_exporter:9100"]

  - job_name: 'cadvisor'
    static_configs:
      - targets: ["cadvisor:9091"]
```

`compose.yml` 
```yml
volumes:
  prometheus-data:
    driver: local

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    restart: unless-stopped
    ports:
      - "9090:9090"
    extra_hosts:
      - host.docker.internal=host-gateway
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus

  node_exporter:
    image: quay.io/prometheus/node-exporter:v1.8.0
    container_name: node_exporter
    command: "--path.rootfs=/host"
    pid: host
    restart: unless-stopped
    volumes:
      - /:/host:ro,rslave

  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    restart: unless-stopped
    container_name: cadvisor
    # ports:
    #   - 8080:8080
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    devices:
      - /dev/kmsg
    privileged: true
    command:
      - '-allow_dynamic_housekeeping=false'
      - '-housekeeping_interval=15s'
      - '-docker_only=true'
      - '-disable_metrics=disk'
- 
```

```shell
docker compose up -d
```
### Step 3: Web UI
Visit `http://HOSTNAME:9090/targets` and fingers crossed 🤞 that your `http://host.docker.internal:9323/metrics` will have a green "UP" state.

## Step 4: Grafana
hook in the data to grafana by adding a new data source. In my case the Prometheus endpoint `http://HOSTNAME:9090`

### dashboards
No need to create a dashboard from scratch. There are plenty of community configured dashboards that can get you up and running with your Prometheus analytics. Just "import" a new dashboard and enter the `dashboard ID` from any of the found [Grafana Labs Dashboards](https://grafana.com/grafana/dashboards/?search=cadvisor)

> [!warning] Angular depreciation
> Some dashboards panels may give you a warning about "Angular Depreciation". It's an easy fix. Go edit the panel in question and set it's `Visualization Type` "Graph" to the modern "Time series" instead.

Here are a few that I found useful
- [Cadvisor exporter | Grafana Labs](https://grafana.com/grafana/dashboards/14282-cadvisor-exporter/)
- [Docker monitoring | Grafana Labs](https://grafana.com/grafana/dashboards/14841-docker-monitoring/)
- [Kubernetes Monitoring Dashboard | Grafana Labs](https://grafana.com/grafana/dashboards/12740-kubernetes-monitoring/)

---
## Credits
- [Installation | Prometheus](https://prometheus.io/docs/prometheus/latest/installation/)
- [Collect Docker metrics with Prometheus | Docker Docs](https://docs.docker.com/config/daemon/prometheus/)
- [Server Monitoring // Prometheus and Grafana Tutorial (youtube.com)](https://www.youtube.com/watch?v=9TJx7QTrTyo)
- [How to share docker metrics with Prometheus using Ubuntu server Snap Docker Service - Server Fault](https://serverfault.com/questions/1151857/how-to-share-docker-metrics-with-prometheus-using-ubuntu-server-snap-docker-serv)
- [Effortless Server Monitoring: Install Grafana, Prometheus & Node Exporter with Docker! - YouTube](https://www.youtube.com/watch?v=yrscZ-kGc_Y)
- [Real-time Docker Container Monitoring with Prometheus and Grafana | by Chao Geng | Mar, 2024 | DevOps.dev](https://blog.devops.dev/real-time-docker-container-monitoring-with-prometheus-and-grafana-498078334a99)
- [linux - Create container cadvisor An error occurred mountpoint for cpu not found - Stack Overflow](https://stackoverflow.com/questions/72282775/create-container-cadvisor-an-error-occurred-mountpoint-for-cpu-not-found)
- [(Relatively) high CPU usage for the cadvisor container · Issue #2523 · google/cadvisor (github.com)](https://github.com/google/cadvisor/issues/2523)