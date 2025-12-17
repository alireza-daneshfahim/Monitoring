# Monitoring Stack with Prometheus, Grafana, Nginx, and cAdvisor

This repository provides a simple yet production-ready monitoring stack based on **Prometheus**, **Grafana**, **cAdvisor**, and **Nginx** as a reverse proxy.

The stack is designed to be clean, understandable, and suitable for publishing on GitHub.

---

## Architecture Overview

* **Prometheus**: Metrics collection and time-series database
* **cAdvisor**: Docker host and container metrics exporter
* **Grafana**: Visualization and dashboards
* **Nginx**: Reverse proxy and access control (Basic Auth for Prometheus)
* **Docker Compose**: Service orchestration

All services communicate over an isolated Docker bridge network called `monitoring`.

---

### Prerequisites
- Docker
- Docker Compose
- apache2-utils installed on the **host system**


## HTTP Basic Authentication (Important)

To protect **Prometheus**, HTTP Basic Authentication is enabled via Nginx.

### Requirements on the Host Machine

You **must install `apache2-utils` on your local machine** in order to generate the `.htpasswd` file:

```bash
sudo apt update
sudo apt install apache2-utils
```

### Create Credentials

```bash
mkdir -p auth
htpasswd -c auth/.htpasswd admin
```

* You will be prompted to enter a password
* The generated `.htpasswd` file is mounted **read-only** into the Nginx container

No credentials are baked into the Docker image itself.

---
### Startup

```bash
docker compose up -d --build
```


## Adding Prometheus as a Data Source in Grafana

1. Open Grafana in your browser:

   ```
   http://grafana.ceph01.ir
   ```

2. Login (default credentials if unchanged):

   * Username: `admin`
   * Password: `admin`

3. Navigate to:
   **Settings → Data Sources → Add data source**

4. Select **Prometheus**

5. Configure the data source:

   * **URL**: `http://prometheus:9090`
   * **Access**: Server (default)

6. Click **Save & Test**

You should see a successful connection message.

---

## Adding Docker Host Dashboard via cAdvisor

### Step 1: Import Dashboard

1. In Grafana, go to:
   **Dashboards → Import**

2. Use one of the well-known cAdvisor dashboards:

   * **Dashboard ID**: `193`
   * (Alternative popular IDs: `14282`, `179`)

3. Click **Load**

### Step 2: Select Data Source

* Choose your **Prometheus** data source
* Click **Import**

You will now see:

* Docker host CPU, memory, disk usage
* Per-container resource consumption

---

## Notes

* Nginx acts as a single entry point for Grafana and Prometheus
* Prometheus is protected with Basic Auth
* Grafana is exposed without auth at Nginx level (Grafana has its own auth)
* cAdvisor should not be exposed publicly in production

---

## License

MIT (or your preferred license)

