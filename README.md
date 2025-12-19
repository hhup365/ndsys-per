# EdgeLink System Daemon (ESD)

A lightweight, containerized agent for distributed system telemetry, edge ingress management, and secure link aggregation. Designed for multi-cloud monitoring and orchestrating microservices connectivity.

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/) [![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE) [![Docker](https://img.shields.io/badge/container-ready-blue)](https://hub.docker.com/)

## Key Features

*   **APM Telemetry**: Real-time performance metrics collection and reporting.
*   **Edge Ingress**: Secure tunnel establishment for internal service exposure.
*   **Link Aggregation**: Configurable upstream gateway routing.
*   **Auto-Discovery**: Automatic service registration and health checks.

## Quick Start

### Docker Deployment

Deploy the agent with a single command. Replace the environment variables with your specific configuration.

```bash
docker run -d \
  --name esd-node \
  --restart always \
  --network host \
  -e UUID="your-guid-here" \
  -e KOMARI_HOST="https://monitor.example.com" \
  -e KOMARI_TOKEN="your-access-token" \
  -e ARGO_AUTH="eyJh..." \
  ghcr.io/YOUR_USERNAME/REPO_NAME:latest
```

## Configuration Reference

The agent is fully configurable via Environment Variables.

### 1. Identity & Telemetry (APM)
Configuration for the Application Performance Monitoring (APM) subsystem.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `UUID` | *Random* | **Instance ID**. Unique identifier for this agent node. |
| `KOMARI_HOST` | - | **Telemetry Endpoint**. The remote server URL to receive status reports (e.g., CPU/RAM usage). |
| `KOMARI_TOKEN` | - | **Telemetry Key**. API token for authenticating with the telemetry endpoint. |
| `NAME` | - | **Node Label**. A human-readable tag for this instance (e.g., `US-West-01`). |

### 2. Edge Ingress (Tunneling)
Settings for the secure ingress controller.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `ARGO_AUTH` | - | **Tunnel Credentials**. JSON blob or Token for the edge tunnel connection. If empty, a temporary tunnel is initialized. |
| `ARGO_DOMAIN`| - | **Fixed Hostname**. The reserved domain name for this tunnel (requires `ARGO_AUTH`). |
| `ARGO_PORT` | `8001` | **Internal Binding**. Local port used by the ingress controller to forward traffic. |

### 3. Upstream Routing (Gateway)
Configuration for upstream load balancing and relay optimization.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `CFIP` | `cdns.doon.eu.org` | **Gateway Address**. The IP or hostname of the upstream relay gateway. |
| `CFPORT` | `443` | **Gateway Port**. The port to connect to on the upstream gateway. |

### 4. Service Registry & Sync
Options for automatic configuration synchronization and service discovery.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `UPLOAD_URL` | - | **Registry Endpoint**. URL to synchronize the local instance configuration map. |
| `PROJECT_URL`| - | **Service URL**. The public-facing URL of this deployment (used for self-registration). |
| `SUB_PATH` | `sub` | **Config Path**. The URI path where the instance configuration is exposed. |
| `AUTO_ACCESS`| `false` | **Health Check**. Set to `true` to enable periodic self-healing requests to the `PROJECT_URL`. |

### 5. System Runtime

| Variable | Default | Description |
| :--- | :--- | :--- |
| `SERVER_PORT`| `3000` | **Management Port**. Local HTTP port for the status dashboard and health probes. |
| `FILE_PATH` | `./tmp` | **Work Directory**. Path for storing temporary runtime assets and logs. |

## Logs & Diagnostics

The system operates in **silent mode** by default to reduce log volume. To view startup events:

```bash
docker logs esd-node
```

> **Note**: Sensitive information (such as config snapshots) is encoded in Base64 within the logs under system debug tags (e.g., `NET_IO`, `MEM_DUMP`) to prevent accidental leakage in plaintext monitoring tools.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
