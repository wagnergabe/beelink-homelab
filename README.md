# Beelink Home Lab

A documented home-lab environment built to practice infrastructure administration, virtualization, containerized services, storage management, network troubleshooting, and secure remote access.

> Public documentation only. Internal IP addresses, credentials, hostnames, share paths, and other sensitive configuration details have been sanitized.

## Goals

- Build practical experience beyond certification labs
- Host useful self-managed services for personal and small-business workflows
- Practice Linux administration, Docker, networking, storage, troubleshooting, and documentation
- Create a repeatable, maintainable environment rather than a one-off setup

## Architecture Overview

```text
Home Network
    |
Omada Network
    |
Proxmox VE Host
    |
Debian LXC Container
    |
Docker Services: Nextcloud, Jellyfin, Sonarr, Radarr, Portainer, Uptime Kuma
    |
External 20 TB Storage
```

## Hardware and Platform

| Component | Role |
|---|---|
| Beelink mini PC | Physical home-lab host |
| Proxmox VE | Virtualization platform |
| Debian LXC | Docker workload container |
| External 20 TB drive | Shared storage, media, backups, and project files |
| TP-Link Omada | Home network infrastructure |

## Services

| Service | Purpose |
|---|---|
| Proxmox VE | Virtualization host and LXC management |
| Docker | Container runtime |
| Nextcloud | File sharing and client-delivery workflow |
| Samba | Windows-accessible file shares |
| Jellyfin | Personal media and drone-footage playback |
| Sonarr / Radarr | Media-library management |
| Portainer | Docker container management |
| Uptime Kuma | Service monitoring |
| Tailscale | Encrypted remote access without router port forwarding |

## Storage Design

The external drive supports separate workflows for business files, drone projects, media, backups, and templates. It is mounted on the host and passed into the Docker LXC for services that need access to shared storage.

Key design considerations:

- Shared storage is accessible from Windows through Samba.
- Nextcloud provides browser-based file access and controlled sharing.
- Jellyfin reads from dedicated media folders.
- Server configuration backups are stored separately from application data.
- Existing drone footage is preserved while the newer folder structure is gradually adopted.

## Networking and Remote Access

The lab uses an Omada-managed home network and Tailscale for encrypted remote administration.

- No router port forwarding is required for remote administration.
- Tailscale access to the virtualization host was validated with connectivity testing.
- A VPN/DNS conflict was identified and resolved during setup, reinforcing the importance of validating adapter priority, DNS behavior, and service-level connectivity.

## Troubleshooting Highlights

A few real-world issues worked through during this build:

- Configuring Samba shares and mapping them as Windows drive letters
- Passing external storage into an unprivileged LXC container
- Handling ownership and permission limitations with exFAT-backed storage
- Diagnosing Tailscale authentication failures caused by a competing VPN/DNS adapter
- Validating service reachability with command-line networking tools
- Organizing Docker bind mounts so media services use consistent paths

## Skills Demonstrated

- Linux administration
- Proxmox VE and LXC containers
- Docker Compose and container operations
- Samba file sharing
- Storage mounts and permissions
- Basic network troubleshooting
- DNS troubleshooting
- VPN and secure remote-access configuration
- Service documentation and operational runbooks
- Python Scripting

## Planned Improvements

- [ ] Add a sanitized network diagram
- [ ] Document monitoring checks and alerting
- [ ] Create a backup and restore test plan
- [ ] Add a client-facing delivery portal for ZephyrVisions.com
