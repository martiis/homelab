# Docker Compose Stacks

This directory contains Docker Compose configurations for various homelab services. Each stack is self-contained with its own `docker-compose.yml` and `README.md` file.

## Available Stacks

| Stack | Description | Documentation |
| --- | --- | --- |
| **[Traefik](traefik/README.md)** | Reverse proxy and load balancer for automatic service discovery and routing | [README](traefik/README.md) |
| **[Pirateship](pirateship/README.md)** | Complete media automation stack (Plex, Jellyfin, Sonarr, Radarr, Lidarr, Bazarr, Deluge, Jackett, Homer) | [README](pirateship/README.md) |
| **[Pi-hole](pihole/README.md)** | DNS sinkhole for network-wide ad blocking and local DNS resolution | [README](pihole/README.md) |
| **[Prometheus](prometheus/README.md)** | Monitoring stack with Prometheus, Grafana, cAdvisor, and node_exporter | [README](prometheus/README.md) |
| **[Filebrowser](filebrowser/README.md)** | Web-based file manager for uploading, editing, and managing files | [README](filebrowser/README.md) |

## Prerequisites

These scripts require Docker and Docker Compose to be installed on your system.

### Install Docker on Linux

How to [Install Docker Engine](https://docs.docker.com/engine/install/ubuntu/)

**Recommended**: [Docker post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/) (adds your user to the docker group, enables Docker on boot, etc.)

### Install Portainer (Optional)

Portainer provides a web UI for managing Docker containers and stacks. How to [Install Portainer](https://docs.portainer.io/start/install/server/docker/linux)

## Stack Dependencies

- **Pirateship** requires **Traefik** to be running
- Most stacks use Traefik for reverse proxy routing (except services using host networking)
- Ensure Pi-hole is configured if using local domains

## Usage

### Using Docker Compose

Navigate to the stack directory and run:

```bash
docker compose up -d
```

To stop a stack:

```bash
docker compose down
```

### Using Portainer

1. Go to Portainer web UI: `Stacks -> + Add Stack`
2. Name the stack (e.g., `traefik`, `pirateship`)
3. Paste the `docker-compose.yml` contents into the editor
4. Load environment variables from `.env` file if required
5. Deploy the stack

## Upgrade Portainer CE

To upgrade Portainer using Docker on Linux:

```bash
docker stop portainer
docker rm portainer
docker pull portainer/portainer-ce:latest
docker run -d -p 9000:9000 -p 9443:9443 --name=portainer --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

## Environment Variables

Many stacks require environment variables to be set. Create a `.env` file in each stack directory with the required variables. Common variables include:

- `PUID` - User ID for file permissions
- `PGID` - Group ID for file permissions
- `TZ` - Timezone (e.g., `America/New_York`)
- `SRV_DIRECTORY` - Path to service data directory
- `LIBRARY_DIRECTORY` - Path to media library directory

## Notes

- Each stack is independent and can be deployed separately
- Some stacks create external networks/volumes that may be shared
- Check individual stack READMEs for specific prerequisites and configuration
- Traefik should typically be deployed first as it provides routing for other services
