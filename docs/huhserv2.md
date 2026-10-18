# HuHServ2 — Containerized Applications and Service Infrastructure

> **Project type:** Personal homelab  
> **Environment:** Physical x86_64 server  
> **Snapshot date:** 18 October 2026  
> **Scope:** System inventory and observed running services; this is not a production availability claim.

## 1. Overview

HuHServ2 is the application-oriented node of my two-server Linux homelab. It runs multiple self-hosted web applications, supporting services, databases, media applications, and custom workloads using Docker.

The project demonstrates how a wide range of containerized services can be organized and administered on modest physical hardware. It does **not** claim benchmarked capacity, high availability, or production-grade reliability.

## 2. Hardware and Operating System

| Component | Observed configuration |
|---|---|
| CPU | Intel Celeron G540 @ 2.50 GHz |
| CPU architecture | x86_64 |
| Logical CPUs | 2 |
| Memory | 15 GiB reported by `free -h` |
| Operating system | Debian GNU/Linux 12 (bookworm) |
| Kernel | `6.12.95+deb12-amd64` |
| Docker Engine | 29.9.0 |

### Storage layout

| Purpose | Disk size reported by `lsblk` | Filesystem |
|---|---:|---|
| Data storage | 298.1G | ext4 |
| System/root disk | 232.9G (231.9G root partition) | ext4 |
| Swap partition | 976M | swap |

At the time of the snapshot, `free -h` reported **9.1 GiB available memory**. This is only a point-in-time memory observation; it is not a performance or load-test result.

## 3. Docker Workloads

**Observed running containers: 42** (at the time of inventory collection).

Representative application groups found in the live Docker inventory:

| Area | Examples observed |
|---|---|
| Design and collaboration | Penpot, HedgeDoc, ClassQuiz |
| Tasks and personal tools | Vikunja, Actual Budget, PairDrop |
| Media and streaming | Navidrome, Feishin, Owncast, Dispatcharr, qBittorrent, MeTube |
| Dashboards and monitoring | Homepage, Glances |
| Databases and supporting components | PostgreSQL, Valkey, Meilisearch |
| Remote connectivity | Tailscale container, AmneziaWG container |
| Custom software | Multiple locally named application, worker, and web containers (identifiers withheld) |

Some products use several containers, for example an application plus database, proxy, worker, cache, or search service. Therefore, **42 containers does not mean 42 separate applications**.

All listed containers were reported as **running** by `docker ps`. Selected workloads had healthy Docker health-check statuses. The inventory does not establish whether every application is reachable or functioning correctly.

## 4. Administration Experience Demonstrated

- Operating a Debian-based physical server used for multiple containerized workloads.
- Inspecting container state, images, and health-check indicators.
- Working with application stacks that contain separate front-end, back-end, database, and support containers.
- Maintaining a mixture of self-hosted applications and custom services in one environment.
- Reviewing system resources and local storage configuration.

This is a personal infrastructure case study, not a claim of commercial customer delivery.

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

- [ ] Capture sanitized screenshots of container-management and monitoring views.
- [ ] Document one application stack with a sanitized Compose example.
- [ ] Verify the OpenMediaVault installation and version.
- [ ] Measure per-container resource usage under normal workload.
- [ ] Verify and document backups, restore tests, updates, and security controls.
- [ ] Record a real troubleshooting example with initial symptoms, diagnosis, fix, and verification.

## 6. Security and Disclosure

Private addresses, filesystem UUIDs, internal paths, credentials, tokens, access keys, domain names, and potentially identifying custom project names are intentionally excluded. The presence of a running VPN container is not a claim that external connectivity was independently verified.

---

**Maintainer:** [Vyacheslav Sadovsky (MrHuH)](https://github.com/sadovskijslava)  
**Repository:** [homelab-infrastructure](https://github.com/sadovskijslava/homelab-infrastructure)
