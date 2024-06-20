# Server-monitoring-using-docker-compose

Docker Compose Version: Specifies the version of Docker Compose being used (3.8).
Volumes: Creates named volumes (prometheus-data and grafana-data) to persist data for Prometheus and Grafana.
Services
1. Node Exporter
Service Name: node_exporter
Image: quay.io/prometheus/node-exporter:latest - The latest version of Node Exporter.
Container Name: node_exporter
Command:
--path.rootfs=/host - Sets the root filesystem path.
Network Mode: host - Uses the host network stack.
PID Mode: host - Shares the PID namespace with the host.
Restart Policy: unless-stopped - Restarts the container unless it is explicitly stopped.
Volumes:
/:/host:ro,rslave - Mounts the root filesystem of the host into the container as read-only.
Node Exporter collects hardware and OS metrics from the host system.

2. Prometheus
Service Name: prometheus
Image: prom/prometheus:latest - The latest version of Prometheus.
Container Name: prometheus
Ports:
9090:9090 - Maps port 9090 on the host to port 9090 in the container.
Restart Policy: unless-stopped - Restarts the container unless it is explicitly stopped.
Volumes:
./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml - Mounts the Prometheus configuration file from the host.
prometheus-data:/prometheus - Uses the named volume for storing Prometheus data.
Command:
--web.enable-lifecycle - Enables the web lifecycle.
--config.file=/etc/prometheus/prometheus.yml - Specifies the Prometheus configuration file.
Prometheus collects and stores metrics from Node Exporter.

3. Grafana
Service Name: grafana
Image: grafana/grafana:latest - The latest version of Grafana.
Container Name: grafana
Ports:
3000:3000 - Maps port 3000 on the host to port 3000 in the container.
Restart Policy: unless-stopped - Restarts the container unless it is explicitly stopped.
Volumes:
grafana-data:/var/lib/grafana - Uses the named volume for storing Grafana data.
Grafana provides a web interface to visualize the metrics collected by Prometheus.

Summary
Node Exporter: Collects system metrics and makes them available to Prometheus.
Prometheus: Scrapes metrics from Node Exporter and stores them.
Grafana: Visualizes the metrics stored in Prometheus.
