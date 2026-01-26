# filebrowser

Provides a file managing web interface and it can be used to upload, delete, preview, rename and edit files. Also check their [GitHub repository][filebrowser].

## Services

| Name | Image | Port | Access |
| --- | --- | --- | --- |
| filebrowser | filebrowser/filebrowser:s6 | 80 (internal) | http://filebrowser.local.box (via Traefik) |

## Prerequisites

### Traefik

This stack requires [Traefik](../traefik/README.md) to be running first. Traefik provides reverse proxy functionality for accessing filebrowser via `filebrowser.local.box`.

Ensure Traefik is deployed and the `traefik_traefik_net` network exists. If your Traefik uses a different network name, update the network references in `docker-compose.yml` accordingly.

### Environment Variables

Create a `.env` file in the same directory with the following variables:

```
PUID=<your-user-id>
PGID=<your-group-id>
SRV_DIRECTORY=/path/to/directory/to/manage
```

## Run

### Portainer

 - Go to portainer web user interface `Stacks -> + Add Stack`
 - Name the stack whatever you want, but for simplicity - `filebrowser`
 - Paste `docker-compose.yml` file contents to a web editor textarea.
 - Load environment variables from `.env` file.
 - Deploy the stack.

### Docker

We'll be using docker compose and the command is very simple:

```bash
docker compose up -d
```

## Access

Filebrowser is accessible through Traefik reverse proxy at `http://filebrowser.local.box`. Default login credentials are `admin/admin`.

**Note**: For Traefik routing to work, ensure your local DNS (e.g., Pi-hole) resolves `filebrowser.local.box` to your host IP, or add an entry to `/etc/hosts`:

```
<host-ip> filebrowser.local.box
```

## Volumes

- `filebrowser_data` - Configuration and settings
- `filebrowser_db` - Database for file metadata
- `${SRV_DIRECTORY}` - Directory to manage (mounted at `/srv` in container)

## Notes

- **Traefik Dependency**: This stack requires [Traefik](../traefik/README.md) to be running for the `filebrowser.local.box` domain to work
- **Default Credentials**: Change the default `admin/admin` credentials after first login for security
- **Directory Access**: The directory specified in `SRV_DIRECTORY` will be accessible and manageable through the web interface
- **Data Persistence**: Configuration and database are stored in Docker volumes and persist across container restarts

[filebrowser]: https://github.com/filebrowser/filebrowser
