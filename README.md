# Prometheus Setup

- docker port is your-port: img-port

## Docker (Windows)

https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/

### Setup volume
```bash
$ docker volume create prometheus-volume
```

### Create prometheus config file
Make sure you setup Windows exporter (for windows servers) or Node exporter (for linux server)

Make sure the ports and IP are publicly accessible

```yml
global:
  scrape_interval: 30s
  evaluation_interval: 25s

scrape_configs:
    - job_name: "prometheus"
    static_configs:
        - targets: ['localhost:9090']
        
    - job_name: "WMI Exporter"
    static_configs:
        # for windows server on .58
        - targets: ['192.168.0.58:9182']
```

### Spin instance
In the same folder where the `prometheus.yml` is, run the command to spin

- Path to config folder
- e.g `C:/Users/DEVELOPER/Documents/DONN/Personal/Projects/DevOps/docker/promethues_grafana/config`

```bash
$ docker run  --name my-prometheus -p 9090:9090 -v <PATH>:/etc/prometheus prom/prometheus
```


## Setup GRAFANA
### Create a volume (reccommended)
```bash
$ docker volume create grafana-storage
```

### Spin instance
```bash
$ docker run -d -p 3000:3000 --name=grafana --volume grafana-storage:/var/lib/grafana grafana/grafana-oss
```

```bash
$ docker run -d -p 3000:3000 --name=grafana \
  --volume grafana-storage:/var/lib/grafana \
  grafana/grafana-oss
```

## Troubleshooting
- Prometheus (docker) not connecting to Grafana

To access from prometheus docker, use ip: http://host.docker.internal:9090

https://github.com/grafana/grafana/issues/46434

## Setup Node Exporter for LINUX
https://www.fosstechnix.com/install-prometheus-and-grafana-on-ubuntu/


### Installation
v* - version
```bash
$ wget https://github.com/prometheus/node_exporter/releases/download/v*/node_exporter-*.*-amd64.tar.gz
tar xvfz node_exporter-*.*-amd64.tar.gz
cd node_exporter-*.*-amd64
./node_exporter
```

e.g for v1.6.1

```bash
$ wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz

$ tar xvfz node_exporter-1.6.1.linux-amd64.tar.gz

$ cd node_exporter-1.6.1.linux-amd64.tar.gz

$ ./node_exporter
```