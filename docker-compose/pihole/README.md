# pi-hole

Provides a [DNS sinkhole][dns-sinkhole] that protects your devices from unwanted content without installing any client-side software. Also provides local DNS which allows us to have pretty URLs in the local network.

## Services

| Name | Image | Port |
| --- | --- | --- |
| pihole | pihole/pihole:latest | 53/tcp, 53/udp, 8888 |

## Run

Before running a pi-hole in a container, make sure to disable the operating system's DNS resolver if present. 

How to disable Ubuntu DNS resolver:
```
> sudo systemctl disable systemd-resolved.service

> sudo systemctl stop systemd-resolved.service
```

How to disable it on other Linux distributions can be found using Google.

### Portainer

 - Go to portainer web user interface `Stacks -> + Add Stack`
 - Name the stack whatever you want, but for simplicity - `pihole`
 - Paste `docker-compose.yml` file contents to a web editor textarea.
 -  Load environment variables from the `.env` file.
 -  Deploy the stack.

### Docker

We'll be using docker compose and the command is very simple:

```
> docker compose up -d
```

## Access

Open your web browser and go to `http://<host-ip>:8888/admin`. The login password is randomly generated and can be found in container logs.

## Ad lists

Pi-hole comes with one preconfigured list, which already contains thousands of hosts, but there are more and even larger lists from [firebog.net][firebog]. I usually add everything from `https://v.firebog.net/*`.

After adding more lists don't forget to update gravity:
```
Tools -> Update Gravity -> (Click) Update
```

## Use

Configure your router’s DHCP options to force clients to use Pi-hole as their DNS server.

[dns-sinkhole]: https://en.wikipedia.org/wiki/DNS_sinkhole
[firebog]: https://firebog.net/
