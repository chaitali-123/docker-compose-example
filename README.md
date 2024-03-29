## Compose sample
### Prometheus & Grafana

Project structure:
```
.
├── compose.yaml
|-- node-exporter
|   | -- expose
├── grafana
│   └── datasource.yml
├── prometheus
│   └── prometheus.yml
└── README.md
```

[_compose.yaml_](compose.yaml)
```
services:
  node-exporter:
    image: prom/node-exporter:latest
    expose:
      - 9100 
  prometheus:
    image: prom/prometheus
    ...
    ports:
      - 9090:9090
  grafana:
    image: grafana/grafana
    ...
    ports:
      - 3000:3000
```
The compose file defines a stack with three services `prometheus`, `node-exporter` and `grafana`.
When deploying the stack, docker compose maps port the default ports for each service to the equivalent ports on the host in order to inspect easier the web interface of each service.
Make sure the ports 9090 and 3000 on the host are not already in use.

## Deploy with docker compose

```
$ docker compose up -d
[+] Running 3/3
 ✔ Container node-exporter  Started                                                                                                                                                                    0.0s
 ✔ Container prometheus     Started                                                                                                                                                                    0.0s
 ✔ Container grafana        Started
```

## Expected result

Listing containers must show two containers running and the port mapping as below:
```
$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED       STATUS          PORTS                    NAMES
b7d23860a102   prom/node-exporter:latest   "/bin/node_exporter …"   2 hours ago   Up 26 seconds   9100/tcp                 node-exporter
0caedc99d84d   prom/prometheus             "/bin/prometheus --c…"   2 hours ago   Up 26 seconds   0.0.0.0:9090->9090/tcp   prometheus
ff73c2b18acb   grafana/grafana             "/run.sh"                2 hours ago   Up 26 seconds   0.0.0.0:3000->3000/tcp   grafana
chaitali@FVFDVK2UQ05F test %
```

Navigate to `http://localhost:3000` in your web browser and use the login credentials specified in the compose file to access Grafana. It is already configured with prometheus as the default datasource.

![page](output.jpg)

Navigate to `http://localhost:9090` in your web browser to access directly the web interface of prometheus.

Stop and remove the containers. Use `-v` to remove the volumes if looking to erase all data.
```
$ docker compose down -v
```
