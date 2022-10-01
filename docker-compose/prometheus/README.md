# Prometheus stack

Use it to monitor host and docker container resource usage. Metrics are exported using [cAdvisor][cadvisor] and [node_exporter][node_exporter]. Collected by [prometheus][prometheus] and visualized in [grafana][grafana].

## Services

| Name | Image | Port |
| --- | --- | --- |
| prometheus | prom/prometheus:latest | 9090 |
| prometheus-exporter-cadvisor | gcr.io/cadvisor/cadvisor:v0.45.0 | 9089 |
| prometheus-exporter-node | quay.io/prometheus/node-exporter:latest | 9088 |
| grafana | grafana/grafana-oss | 9080 |

## Run

Because Prometheus is not using the host network for some magical reason it can only reach exporters through the host IP. In file `etc/prometheus/prometheus.yml` replace `<host-ip>` placeholder to a host IP on which docker is running. An address can be found by running `ifconfig` in a terminal. Usually, search for adapter `eno*` or `eth*` and `inet` value should be similar to `192.168.x.x` - it's your host IP.

### Portainer

We assume that you already have the portainer up and running. Use portainer's stacks feature which allows deploying docker-compose files.

 - If portainer is running on a remote host copy `etc/prometheus` directory on it
 - Go to portainer web user interface `Stacks -> + Add Stack`
 - Name the stack whatever you want, but for simplicity - `prometheus`
 - Paste `docker-compose.yml` file contents to a web editor textarea.
 -  Add an environment variable `PROMETHEUS_YML_FILEPATH` and set its value file path where `prometheus.yml` is located on the host.
 -  Deploy the stack.

### Docker

We'll be using docker compose and the command is very simple:

`docker compose up -d`

### Post Run

#### Check health

Exporter health can be checked here: `http://<host-ip>:9090/targets`. All states should be up and green.

#### Setup grafana.

 - Open `http://<host-ip>:9080`. Default login is `admin/admin`.
 - Create a new data source: `Configuration -> Data sources -> Add data source -> Prometheus`. Input URL: `http://prometheus:9090`, uncheck toggle `Manage alerts via Alerting UI`, save and test. It should say that `Data source is working`.
 - Import dashboards - `Dashboards -> Import`. Let's import via json. Dashboards are located in `etc/grafana/dashboards`. Copy, paste json contents and click Load. Select prometheus data source and click Import.


[cadvisor]: https://github.com/google/cadvisor
[node_exporter]: https://github.com/prometheus/node_exporter
[prometheus]: https://github.com/prometheus/prometheus
[grafana]: https://github.com/grafana/grafana
