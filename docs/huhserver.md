# HuHServer — Linux Storage and Self-Hosted Services

> **Project type:** Personal homelab  
> **Environment:** Physical x86_64 server  
> **Snapshot date:** 18 October 2026  
> **Scope:** System inventory and observed running services; this is not a production availability claim.

## 1. Overview

HuHServer is part of my two-server Linux homelab. It hosts network storage and self-hosted applications, including a personal cloud, media services, container management, and system monitoring.

The server provides hands-on experience with Linux administration, storage organization, Docker deployment, and application troubleshooting.

## 2. Hardware and Operating System

| Component | Observed configuration |
|---|---|
| CPU | Intel Pentium G620 @ 2.60 GHz |
| CPU architecture | x86_64 |
| Logical CPUs | 2 |
| Memory | 15 GiB reported by `free -h` |
| Operating system | Debian GNU/Linux 12 (bookworm) |
| Kernel | `6.12.95+deb12-amd64` |
| Docker Engine | 29.4.0 |

### Storage layout

| Purpose | Disk size reported by `lsblk` | Filesystem |
|---|---:|---|
| Data storage | 465.8G | ext4 |
| System/root disk | 298.1G (297.1G root partition) | ext4 |
| Swap partition | 976M | swap |

The storage inventory was taken from `lsblk`. Disk health, redundancy, free capacity, and backup coverage have not yet been verified as part of this case study.

## 3. Docker Workloads

**Observed running containers: 7** (at the time of inventory collection).

| Container | Role |
|---|---|
| `jellyfin` | Self-hosted media server |
| `nextcloud` | Personal cloud and file access |
| `portainer` | Web-based container management |
| `glances` | System monitoring |
| `homebox` | Inventory / asset management application |
| `tunarr` | Media channel management |
| `halcyon` | Self-hosted video application |

All seven containers were reported as **running** by `docker ps`. Some reported Docker health checks as healthy. Container runtime status alone does not verify full application functionality, performance, or availability over time.

## 4. Administration Experience Demonstrated

- Operating and inspecting a Debian-based physical server.
- Reviewing Linux CPU, memory, kernel, filesystem, and disk information.
- Hosting multiple applications as Docker containers.
- Working with persistent storage and self-hosted application services.
- Inspecting container images, uptime, and health status.

These points describe the personal homelab work and the system inventory supplied for this case study; they are not presented as paid customer engagements.

## 5. Evidence and Verification

The snapshot was collected with read-only commands:

```bash
cat /etc/os-release
uname -r
lscpu
free -h
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
docker --version
docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}'
```

### Next documentation tasks

- [ ] Add sanitized screenshots from Portainer and the server dashboard.
- [ ] Document an example deployment, without secrets or live credentials.
- [ ] Verify the OpenMediaVault installation and version.
- [ ] Document backup and restore procedures after testing them.
- [ ] Add measurements for disk utilization, container resource usage, and operational uptime.

## 6. Security and Disclosure

Internal IP addresses, filesystem UUIDs, user-specific mount paths, credentials, access keys, and unredacted configuration files are intentionally omitted. This document contains an observed technical inventory rather than a security audit or performance benchmark.

---

**Maintainer:** [Vyacheslav Sadovsky (MrHuH)](https://github.com/sadovskijslava)  
**Repository:** [homelab-infrastructure](https://github.com/sadovskijslava/homelab-infrastructure)
