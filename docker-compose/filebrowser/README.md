# filebrowser

Provides a file managing web interface and it can be used to upload, delete, preview, rename and edit files. Also check their [GitHub repository][filebrowser].

## Services

| Name | Image | Port |
| --- | --- | --- |
| filebrowser | filebrowser/filebrowser:s6 | 8880 |

## Run

### Portainer

 - Go to portainer web user interface `Stacks -> + Add Stack`
 - Name the stack whatever you want, but for simplicity - `filebrowser`
 - Paste `docker-compose.yml` file contents to a web editor textarea.
 -  Load environment variables from `.env` file.
 -  Deploy the stack.

### Docker

We'll be using docker compose and the command is very simple:

`docker compose up -d`

## Access

Open your web browser and go to `http://<host-ip>:8880`. Default login credentials are `admin/admin`.

[filebrowser]: https://github.com/filebrowser/filebrowser
