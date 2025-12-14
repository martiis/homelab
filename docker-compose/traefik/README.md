# Traefik

[Traefik][traefik] is a modern reverse proxy and load balancer that makes deploying microservices easy. It automatically discovers and configures routes for Docker containers using labels, eliminating the need for manual configuration.

## Services

| Name | Image | Port | Access |
| --- | --- | --- | --- |
| traefik | traefik:v3 | 80, 8080 | http://<host-ip>:8080 (dashboard) |

## Run

### Portainer

 - Go to portainer web user interface `Stacks -> + Add Stack`
 - Name the stack whatever you want, but for simplicity - `traefik`
 - Paste `docker-compose.yml` file contents to a web editor textarea.
 - Deploy the stack.

### Docker

We'll be using docker compose and the command is very simple:

```bash
docker compose up -d
```

## Access

The Traefik dashboard is accessible at `http://<host-ip>:8080`. The dashboard is configured in insecure mode for local network access.

## Network Configuration

Traefik creates a bridge network `traefik_traefik_net`. This network is used by Traefik's Docker provider to discover and route traffic to containers. Other stacks (like pirateship) must connect their containers to this network for Traefik routing to work.

## Entry Points

Traefik is configured with the following entry points:

- **http** (port 80): Main HTTP entry point for web services
- **remote** (port 27813): Custom entry point for remote access
- **metrics** (port 8082): Metrics endpoint (exposed internally)

## Docker Provider

Traefik is configured to:
- Automatically discover Docker containers
- Only expose containers with `traefik.enable=true` label
- Use the `traefik_net` network (or `traefik_traefik_net` if renamed) for service discovery

## Usage with Other Stacks

To use Traefik with other stacks (e.g., pirateship), ensure:

1. The other stack's containers are on the `traefik_traefik_net` network
2. Containers have appropriate Traefik labels:
   ```yaml
   labels:
     - "traefik.enable=true"
     - "traefik.docker.network=traefik_traefik_net"
     - "traefik.http.routers.<service-name>.entrypoints=http"
     - "traefik.http.routers.<service-name>.rule=Host(`<domain>`)"
     - "traefik.http.services.<service-name>.loadbalancer.server.port=<port>"
   ```

## Notes

- Traefik requires access to the Docker socket (`/var/run/docker.sock`) to discover containers
- The dashboard is enabled in insecure mode - consider adding authentication for production use
- Traefik must be running before other stacks that depend on it

[traefik]: https://traefik.io/

