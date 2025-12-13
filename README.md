# Nginx Reverse Proxy for Prometheus & Grafana

This repository contains a minimal, production-ready monitoring stack based on **Prometheus**, **Grafana**, and **Nginx** as a reverse proxy with basic authentication.

---
## README

### Overview
This stack provides:
- Prometheus for metrics collection
- Grafana for visualization
- Nginx as a reverse proxy
- Basic Authentication for Prometheus endpoint

All services communicate over an isolated Docker bridge network.

---

### Prerequisites
- Docker
- Docker Compose
- apache2-utils installed on the **host system**

Install apache2-utils locally:

```bash
sudo apt update
sudo apt install apache2-utils
```

---

### Setting up Basic Authentication

The htpasswd file **must be generated on the host**, not during image build.

```bash
mkdir -p auth
htpasswd -c auth/.htpasswd admin
```

You will be prompted to enter a password. The resulting file is mounted read-only into the Nginx container and used for Prometheus authentication.

---

### Startup

```bash
docker compose up -d --build
```

---

### Access URLs

- Grafana: http://grafana.ceph01.ir
- Prometheus: http://prometheus.ceph01.ir (Basic Auth protected)

---

### Notes
- Grafana authentication is handled internally by Grafana itself
- Prometheus is protected via Nginx Basic Auth
- htpasswd credentials can be rotated without rebuilding images
- Volumes ensure data persistence across container restarts

---

### Recommended Next Steps
- Enable HTTPS with Certbot
- Add Node Exporter and additional scrape targets
- Configure Grafana dashboards and data sources
- Apply rate limiting and security headers in Nginx
---

## License

MIT

