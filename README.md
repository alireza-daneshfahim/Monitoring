# Overview

This repository contains a minimal, production-oriented monitoring stack based on **Prometheus**, **Grafana**, and **Nginx** running on **Docker Compose**.

Nginx acts as a reverse proxy and exposes Grafana and Prometheus via separate virtual hosts. Prometheus collects metrics, and Grafana provides visualization.

---

## Usage

```bash
docker compose up -d
```

After startup:

* Grafana: `http://grafana.ceph01.ir`
* Prometheus: `http://prometheus.ceph01.ir`

---

## Notes & Recommendations

* Change default Grafana admin credentials in production.
* Add TLS (Let's Encrypt / Certbot) on Nginx for secure access.
* Extend `prometheus.yml` with Node Exporter, cAdvisor, or custom jobs.
* Protect Prometheus endpoint with auth or IP restrictions if exposed publicly.

---

## License

MIT

