# Ubuntu Base Image

Local Ubuntu 24.04 lab container with systemd, SSH, and fixed training accounts. It is not a production image or LAN control plane.

```powershell
.\up.ps1
.\down.ps1
```

The container publishes SSH only on workstation loopback. Read [docs/README.md](docs/README.md) for port, login, direct Docker, and GHCR details.

Do not place production credentials, inventory, or host keys in this repository. Validate Compose before start:

```powershell
docker compose -f container/docker-compose.yml config --quiet
```
