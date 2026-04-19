# AI Observability Lab

Hands-on lab for learning Docker, Docker Compose, Prometheus, Grafana, container monitoring, GPU monitoring, and AI inference observability.

---

## Overview

This repository documents my practical learning journey into AI infrastructure and observability.

The long-term goal of this lab is to build a small but real-world AI observability environment for:

- GPU monitoring
- AI inference service monitoring
- metrics collection
- dashboards
- operational troubleshooting
- infrastructure-focused learning

This project is being built step by step from the ground up so that each layer is understood properly rather than copied blindly.

---

## Current Progress

### Phase 1: Docker Foundations
Completed the initial Docker fundamentals setup and first container lifecycle exercises in WSL.

### Phase 2: Docker Compose + Monitoring Stack
Built a local observability stack using Docker Compose with:

- Prometheus
- Grafana
- node-exporter
- cAdvisor

This stack now provides both:

- **host monitoring** via node-exporter
- **container monitoring** via cAdvisor

---

## What I Have Done So Far

### Docker Fundamentals
- Confirmed Docker is installed and working in WSL
- Pulled and ran the `hello-world` image
- Learned the difference between:
  - images
  - containers
  - running containers
  - exited containers
- Viewed container logs
- Listed running and all containers
- Created a dedicated project directory:
  - `~/ai-observability-lab`
- Ran a real long-lived service container using NGINX
- Published a container port to the host
- Accessed the service successfully in the browser
- Practiced stopping and starting containers
- Removed old stopped containers
- Reviewed local Docker images

### Docker Compose
- Created a multi-service stack with `compose.yaml`
- Learned how to bring a stack up and down with:
  - `docker compose up -d`
  - `docker compose down`
- Learned the difference between:
  - host ports
  - container ports
  - internal Docker networking
- Practiced checking container status with:
  - `docker compose ps`
- Practiced checking service logs with:
  - `docker compose logs`
  - `docker compose logs --tail=30 <service>`

### Prometheus
- Added Prometheus to the Compose stack
- Created a `prometheus.yml` scrape config
- Configured Prometheus to scrape:
  - itself
  - node-exporter
  - cAdvisor
- Used the Prometheus targets page to verify scrape health
- Learned the difference between:
  - browser access via `localhost:<port>`
  - service-to-service access via Docker service names

### Grafana
- Added Grafana to the Compose stack
- Configured Grafana admin credentials through environment variables
- Connected Grafana to Prometheus as a datasource
- Fixed a datasource issue by changing the URL from:
  - `http://localhost:9090`
  - to `http://prometheus:9090`
- Confirmed Grafana can query Prometheus successfully
- Used Grafana Explore to run PromQL queries directly

### node-exporter
- Added node-exporter to collect host-level metrics
- Confirmed host metrics appear in Prometheus
- Imported and viewed a host monitoring dashboard in Grafana
- Observed metrics such as:
  - CPU
  - memory
  - uptime
  - disk
  - network

### cAdvisor
- Added cAdvisor to collect container-level metrics
- Resolved host port conflicts by changing the browser-facing port
- Confirmed cAdvisor UI loads successfully
- Confirmed Prometheus scrapes cAdvisor successfully
- Verified cAdvisor metrics in Grafana with PromQL queries
- Queried container memory and CPU metrics successfully

---

## Current Stack

### Services Running
- Prometheus
- Grafana
- node-exporter
- cAdvisor

### Local Access URLs
- Grafana:
  - `http://localhost:3001`
- Prometheus:
  - `http://localhost:9090`
- cAdvisor:
  - `http://localhost:8088`

### Internal Docker Service Endpoints
These are used **inside the Docker network**:

- Prometheus:
  - `http://prometheus:9090`
- node-exporter:
  - `http://node-exporter:9100`
- cAdvisor:
  - `http://cadvisor:8080`

---

## Concepts Learned So Far

### Docker Image
A Docker image is the blueprint or template used to create containers.

Examples used so far:

- `hello-world`
- `nginx`
- `prom/prometheus`
- `grafana/grafana`
- `prom/node-exporter`
- `gcr.io/cadvisor/cadvisor`

### Docker Container
A container is a running or previously run instance created from an image.

Examples so far include:

- NGINX test containers
- Prometheus container
- Grafana container
- node-exporter container
- cAdvisor container

### Port Publishing
Docker port publishing maps a port on the host machine to a port inside the container.

Examples:

- `3001:3000`
- `9090:9090`
- `8088:8080`

This means:

- the left side is the **host port**
- the right side is the **container port**

### Docker Internal Networking
Containers in the same Compose project can talk to each other using service names.

Examples:

- Grafana reaches Prometheus using:
  - `http://prometheus:9090`
- Prometheus reaches cAdvisor using:
  - `http://cadvisor:8080`

This was an important lesson because `localhost` inside one container does **not** mean another container.

### Running vs Exited Containers
- `docker ps` shows only running containers
- `docker ps -a` shows all containers, including exited ones

### Logs
Container logs help verify startup behavior, scrape status, and troubleshooting information.

Examples:
- checking Grafana startup
- checking cAdvisor startup
- checking Prometheus service health
- checking Compose service behavior after config changes

### `docker stats`
`docker stats` provides a live view of running container resource usage, including:

- CPU usage
- memory usage
- network I/O
- block I/O
- process count

This is useful for comparing:

- raw Docker live stats
- cAdvisor metrics
- Prometheus scrape data
- Grafana dashboards

---

## Commands Practiced

### Docker Basics

#### Docker Version Check
    docker --version

#### Pull Images
    docker pull hello-world
    docker pull nginx

#### Run Containers
    docker run hello-world
    docker run -d --name my-nginx -p 8080:80 nginx

#### View Running Containers
    docker ps

#### View All Containers
    docker ps -a

#### View Container Logs
    docker logs practical_jepsen
    docker logs my-nginx

#### Stop and Start a Container
    docker stop my-nginx
    docker start my-nginx

#### Remove a Stopped Container
    docker rm practical_jepsen

#### View Local Images
    docker images

#### Live Container Resource Usage
    docker stats

#### One-Time Container Resource Snapshot
    docker stats --no-stream

---

### Docker Compose

#### Start the Stack
    docker compose up -d

#### Stop the Stack
    docker compose down

#### Stop the Stack and Remove Volumes
    docker compose down -v

#### View Running Compose Services
    docker compose ps

#### View Logs
    docker compose logs
    docker compose logs --tail=30 prometheus
    docker compose logs --tail=30 grafana
    docker compose logs --tail=30 cadvisor

#### Restart a Single Service
    docker compose restart prometheus

#### Check Config Inside a Running Container
    docker compose exec prometheus cat /etc/prometheus/prometheus.yml

#### Test Reachability Between Containers
    docker compose exec prometheus wget -qO- http://cadvisor:8080/metrics | head

---

## PromQL Queries Tested

### Verify cAdvisor Is Up
    up{job="cadvisor"}

### Raw Container Memory Metric
    container_memory_usage_bytes

### Per-Container Memory Usage
    sum by (id) (
      container_memory_usage_bytes{
        job="cadvisor",
        id=~"/docker/.*"
      }
    )

### Per-Container CPU Usage
    sum by (id) (
      rate(container_cpu_usage_seconds_total{
        job="cadvisor",
        id=~"/docker/.*"
      }[1m])
    )

### Raw Network Receive Metric
    container_network_receive_bytes_total{job="cadvisor"}

### Raw Network Transmit Metric
    container_network_transmit_bytes_total{job="cadvisor"}

### Per-Container Network Receive
    sum by (id) (
      rate(container_network_receive_bytes_total{
        job="cadvisor"
      }[5m])
    )

### Per-Container Network Transmit
    sum by (id) (
      rate(container_network_transmit_bytes_total{
        job="cadvisor"
      }[5m])
    )

---

## Current Repository Purpose

This repository is intended to become the main working area for:

- Docker Compose files
- Prometheus configuration
- Grafana setup
- container monitoring
- future GPU monitoring exporters
- inference monitoring experiments
- observability notes
- troubleshooting notes
- screenshots and documentation

---

## Key Lessons Learned

### 1. `localhost` Means Different Things in Different Places
Inside a container, `localhost` refers to that container itself, not the host machine and not another service.

### 2. Host Ports and Internal Container Ports Are Different
Example:

- Browser uses:
  - `http://localhost:8088`
- Prometheus uses:
  - `http://cadvisor:8080`

### 3. Prometheus UI and API Are Useful for Troubleshooting
The Prometheus targets page and API responses are helpful for confirming whether a service is really being scraped.

### 4. cAdvisor Adds Container Visibility
node-exporter shows host-level metrics, while cAdvisor shows container-level metrics.

### 5. Grafana Explore Is Great for Learning
Running raw PromQL queries in Grafana Explore is a great way to understand what metrics exist before building dashboards.

---

## Current Status

Current status:

- Docker basics complete
- First long-lived container complete
- Port mapping complete
- Container lifecycle basics complete
- Docker Compose basics complete
- Prometheus stack running
- Grafana stack running
- node-exporter scraping successfully
- cAdvisor scraping successfully
- Host monitoring working
- Container monitoring working
- Basic PromQL exploration working

---

## Next Step

### Fix External Service Scraping
The next milestone is to correctly connect Prometheus to external services such as:

- Lighthouse metrics
- Nethermind metrics

These targets are currently down because Prometheus is running inside Docker and cannot scrape host services using `localhost`.

This will require configuring Prometheus to use reachable addresses for those services.

---

## Planned Future Roadmap

### Near-Term
- Clean up project structure
- Add screenshots to the repository
- Add dashboard documentation
- Fix Lighthouse metrics scraping
- Fix Nethermind metrics scraping

### Next Layer
- Add more polished Grafana dashboards
- Improve Prometheus target organization
- Add alerting concepts
- Learn more about exporters and service discovery

### AI Observability Layer
- Add GPU monitoring exporters
- Add a simple inference service
- Track request latency, throughput, and resource usage
- Explore small-team AI inference observability workflows
- Build toward practical AI infra troubleshooting skills

---

## Why This Project Exists

I want to build real skills in:

- Linux
- Docker
- Docker Compose
- monitoring
- observability
- infrastructure troubleshooting
- AI systems operations

The long-term aim is to turn this into a practical AI infra lab focused on:

- GPU monitoring
- inference observability
- metrics pipelines
- dashboards
- reliability workflows

This repo is being built as both:

- a learning project
- a portfolio project

---

## Status Summary

This project has now progressed from:

- running single Docker containers manually

to:

- running a multi-service observability stack with Docker Compose
- collecting host metrics with node-exporter
- collecting container metrics with cAdvisor
- querying metrics in Prometheus
- visualizing metrics in Grafana

This is the current foundation for the next stage of the lab.
