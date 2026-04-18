# AI Observability Lab

Hands-on lab for learning Docker, Prometheus, Grafana, GPU monitoring, and AI inference observability.

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

### What I Have Done So Far

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
- Published a container port to the host with:
  - `8080:80`
- Accessed the service successfully in the browser at:
  - `http://localhost:8080`
- Practiced stopping and starting containers
- Removed an old stopped container
- Reviewed container images stored locally

---

## Concepts Learned So Far

### Docker Image

A Docker image is the blueprint or template used to create containers.

Examples used so far:

- `hello-world`
- `nginx`

### Docker Container

A container is a running or previously run instance created from an image.

Examples used so far:

- `practical_jepsen`
- `my-nginx`

### Port Publishing

Docker port publishing maps a port on the host machine to a port inside the container.

Example:

- Host port `8080`
- Container port `80`

This allowed the NGINX service running inside the container to be accessed from the browser.

### Running vs Exited Containers

- `docker ps` shows only running containers
- `docker ps -a` shows all containers, including exited ones

### Logs

Container logs help verify startup behavior, request handling, and errors.

Examples:

- confirmed NGINX started correctly
- confirmed browser requests reached the container
- observed a normal `favicon.ico` 404 request

---

## Commands Practiced

### Docker Version Check

    docker --version

### Pull Images

    docker pull hello-world
    docker pull nginx

### Run Containers

    docker run hello-world
    docker run -d --name my-nginx -p 8080:80 nginx

### View Running Containers

    docker ps

### View All Containers

    docker ps -a

### View Container Logs

    docker logs practical_jepsen
    docker logs my-nginx

### Stop and Start a Container

    docker stop my-nginx
    docker start my-nginx

### Remove a Stopped Container

    docker rm practical_jepsen

### View Local Images

    docker images

---

## Project Directory

Current project directory:

    /home/saltylighter/ai-observability-lab

This repository is intended to become the main working area for:

- Docker Compose files
- Prometheus configuration
- Grafana setup
- GPU monitoring exporters
- inference monitoring experiments
- observability notes and screenshots

---

## First Real Milestone Completed

### Successfully Ran a Real Service Container with an Exposed Port

NGINX was launched in detached mode and made accessible via localhost.

Command used:

    docker run -d --name my-nginx -p 8080:80 nginx

What this command means:

- `docker run`
  - Create and start a new container
- `-d`
  - Run in detached mode
- `--name my-nginx`
  - Give the container a readable name
- `-p 8080:80`
  - Publish host port `8080` to container port `80`
- `nginx`
  - Use the official NGINX image

Result:

- NGINX image downloaded successfully
- container started successfully
- service reachable in browser
- logs confirmed successful startup and HTTP requests

---

## Notes From Early Learning

### `touch`

Used to create a file.

Example:

    touch README.md

### `nano`

Used to open and edit a file in the Linux terminal.

Example:

    nano README.md

### `ls -la`

Used to list files with detailed information, including hidden files.

Meaning:

- `ls` = list
- `-l` = long format
- `-a` = all files, including hidden ones

### `mkdir -p`

Used to create directories safely.

Meaning:

- `mkdir` = make directory
- `-p` = create parent path if needed and do not fail if it already exists

---

## Why This Project Exists

I want to build real skills in:

- Linux
- Docker
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

## Next Step

### Docker Compose

The next milestone is to move from manually running single containers to defining services in a `docker-compose.yml` file.

That will be the bridge into:

- repeatable infrastructure setup
- multi-service stacks
- Prometheus + Grafana
- future GPU and inference observability components

---

## Planned Future Roadmap

### Near-Term

- Learn Docker Compose
- Rebuild NGINX as a Compose-managed service
- Learn volumes, bind mounts, and networks
- Organize project structure cleanly

### Next Layer

- Add Prometheus
- Add Grafana
- Connect metrics collection
- Build and document dashboard basics

### AI Observability Layer

- Add GPU monitoring exporters
- Add a simple inference service
- Track request latency, throughput, and resource usage
- Explore small-team AI inference observability workflows

---

## Status

Current status:

- Docker basics complete
- First long-lived container complete
- Port mapping complete
- Container lifecycle basics complete
- Ready to begin Docker Compose
