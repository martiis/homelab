# Pirateship

A complete media automation stack for downloading, organizing, and streaming media content. Includes media servers (Plex, Jellyfin), download clients (Deluge), indexers (Jackett, FlareSolverr), and management tools (Sonarr, Radarr, Bazarr) for TV shows and movies. Also includes Homer dashboard for easy access to all services.

## Services

| Name | Image | Port | Access |
| --- | --- | --- | --- |
| homer | b4bz/homer:latest | 8080 | http://homer.local.box |
| flaresolverr | ghcr.io/flaresolverr/flaresolverr:latest | 8191 | http://flaresolverr.local.box |
| bazarr | linuxserver/bazarr:latest | 6767 | http://bazarr.local.box |
| sonarr | linuxserver/sonarr:latest | 8989 | http://sonarr.local.box |
| radarr | linuxserver/radarr:latest | 7878 | http://radarr.local.box |
| deluge | linuxserver/deluge:latest | 8112 | http://deluge.local.box |
| jackett | linuxserver/jackett:latest | 9117 | http://jackett.local.box |
| plex | linuxserver/plex:latest | 32400 | http://<host-ip>:32400/web |
| jellyfin | linuxserver/jellyfin:latest | 8096 | http://<host-ip>:8096 |

## Prerequisites

### Traefik

This stack requires [Traefik](../traefik/README.md) to be running first. Traefik provides reverse proxy functionality for most services in this stack.

Ensure Traefik is deployed and the `traefik_traefik_net` network exists. If your Traefik uses a different network name, update the network references in `docker-compose.yml` accordingly.

### Environment Variables

Create a `.env` file in the same directory with the following variables:

```
PUID=<your-user-id>
PGID=<your-group-id>
TZ=<your-timezone>
SRV_DIRECTORY=/path/to/srv
LIBRARY_DIRECTORY=/path/to/media/library
```

### External Volumes

All data volumes are marked as external and must be created before deployment:

```bash
docker volume create bazarr_data
docker volume create sonarr_data
docker volume create radarr_data
docker volume create deluge_data
docker volume create jackett_data
docker volume create plex_data
docker volume create jellyfin_data
docker volume create lidarr_data
```

### DNS Configuration

For Traefik routing to work, ensure your local DNS (e.g., Pi-hole) resolves `*.local.box` domains to your host IP, or add entries to `/etc/hosts`:

```
<host-ip> homer.local.box flaresolverr.local.box bazarr.local.box sonarr.local.box radarr.local.box deluge.local.box jackett.local.box
```

## Run

### Portainer

 - Go to portainer web user interface `Stacks -> + Add Stack`
 - Name the stack whatever you want, but for simplicity - `pirateship`
 - Paste `docker-compose.yml` file contents to a web editor textarea.
 - Load environment variables from `.env` file.
 - Deploy the stack.

### Docker

We'll be using docker compose and the command is very simple:

```bash
docker compose up -d
```

## Access

Most services are accessible through Traefik reverse proxy at their respective `*.local.box` domains. Plex and Jellyfin use host networking and are accessible directly via host IP:

- **Homer Dashboard**: http://homer.local.box
- **FlareSolverr** (Cloudflare bypass): http://flaresolverr.local.box
- **Bazarr** (Subtitles): http://bazarr.local.box
- **Sonarr** (TV Shows): http://sonarr.local.box
- **Radarr** (Movies): http://radarr.local.box
- **Deluge** (Torrent Client): http://deluge.local.box
- **Jackett** (Indexer): http://jackett.local.box
- **Plex**: http://<host-ip>:32400/web
- **Jellyfin**: http://<host-ip>:8096

## Hardware Acceleration

Plex and Jellyfin are configured with Intel iGPU access via `/dev/dri` device mapping for hardware-accelerated transcoding. Jellyfin also includes render and video group access for additional GPU support.

## Initial Setup

1. **Jackett**: Add indexers/trackers for torrent searching
2. **FlareSolverr**: Configure in Jackett/Radarr/Sonarr if needed for Cloudflare-protected indexers
3. **Deluge**: Configure download paths and settings
4. **Sonarr/Radarr**: 
   - Add download client (Deluge)
   - Add indexer (Jackett, optionally with FlareSolverr)
   - Configure root folders pointing to `/library`
   - Add media to monitor
5. **Bazarr**: Connect to Sonarr/Radarr for subtitle management
6. **Plex/Jellyfin**: 
   - Add media libraries pointing to `/library`
   - Configure users and sharing as needed

## Notes

- **Traefik Dependency**: This stack requires [Traefik](../traefik/README.md) to be running. Most services use Traefik for reverse proxy routing via `*.local.box` domains
- **Host Networking**: Plex and Jellyfin use host networking instead of Traefik for better media streaming performance
- **Media Sharing**: Media files are shared across services via the `LIBRARY_DIRECTORY` volume mount
- **Homer Assets**: Homer dashboard assets are stored in `${SRV_DIRECTORY}/etc/homer/assets`
- **Data Persistence**: All data volumes are external and persist across container restarts

