# HuHServ2 — Containerized Applications and Service Infrastructure

> **Project type:** Personal homelab  
> **Environment:** Physical x86_64 Linux server  
> **Operating system:** Debian GNU/Linux 12 (bookworm)  
> **Initial snapshot:** October 18, 2026  
> **Latest infrastructure inspection:** October 21, 2026  
> **Scope:** System administration, Docker infrastructure, self-hosted applications, private networking, and technical documentation.

---

## 1. Overview

HuHServ2 is the application-oriented node of my two-server Linux homelab.

It hosts self-managed web applications, databases, media services, development tools, monitoring utilities, and custom Docker workloads.

The infrastructure is designed for practical experimentation with Linux administration, container orchestration, application deployment, networking, and troubleshooting.

The project demonstrates experience in:

- Managing a physical Debian-based Linux server.
- Deploying and maintaining Docker containers.
- Working with multi-container application stacks.
- Configuring private remote access through Tailscale.
- Monitoring hardware resources and service availability.
- Organizing self-hosted applications with Homepage.
- Documenting infrastructure configurations and technical investigations.

**This is a personal homelab project, not a commercial production deployment.**

The documentation describes observed configurations and practical administration experience. It does not claim high availability, benchmarked performance, or production-grade reliability.

---

## 2. Hardware and Operating System

The following hardware configuration was recorded during the initial inspection on October 18, 2026.

| Component | Configuration |
|---|---|
| CPU | Intel Celeron G540 @ 2.50 GHz |
| CPU architecture | x86_64 |
| Logical CPUs | 2 |
| Memory | 15 GiB reported by `free -h` |
| Operating system | Debian GNU/Linux 12 (bookworm) |
| Linux kernel | `6.12.95+deb12-amd64` |
| Docker Engine | 29.9.0 (initial snapshot) |
| Server role | Containerized applications and services |

### 2.1. Storage Layout

| Purpose | Observed disk size | Filesystem |
|---|---:|---|
| Data storage | 298.1 GB | ext4 |
| System/root disk | 232.9 GB | ext4 |
| Swap partition | 976 MB | swap |

At the time of the initial snapshot, approximately **9.1 GiB of memory was available**.

These figures represent a point-in-time observation, not a storage benchmark or performance test.

### 2.2. OpenMediaVault Administration

HuHServ2 uses OpenMediaVault for storage and system administration.

The web interface provides access to filesystem information, storage configuration, system statistics, and network services.

**OpenMediaVault Dashboard**

![HuHServ2 OpenMediaVault Dashboard](../screenshots/huhserv2-omv-dashboard.png)

**Filesystem Management**

![HuHServ2 File Systems](../screenshots/huhserv2-filesystems.png)

---

## 3. Docker Infrastructure

Docker is the primary application deployment platform on HuHServ2.

The server hosts both standalone applications and multi-container workloads containing web frontends, backends, databases, workers, and other supporting components.

### 3.1. Container Inventory

The infrastructure has changed over time as applications have been installed, removed, or reorganized.

| Inspection date | Running containers |
|---|---:|
| October 18, 2026 | 42 |
| October 21, 2026 | 25 |

The October 21 inspection identified:

- **25 running containers**
- **9 containers reporting healthy Docker healthchecks**
- Several multi-container application deployments
- A mixture of public container images and locally built images

A running container does not necessarily indicate that the application is fully functional. Containers without a configured healthcheck may report `Up` without additional health information.

### 3.2. Current Workload Categories

| Category | Applications and components |
|---|---|
| Dashboard and monitoring | Homepage, Glances |
| Design and collaboration | Penpot |
| Development infrastructure | Game Dev Hub, application workers |
| Media applications | Navidrome, Feishin |
| File synchronization and sharing | PairDrop, ClipCascade |
| Downloads and automation | qBittorrent, MeTube, Telegram Downloader |
| Networking | Tailscale, AmneziaWG |
| Databases and supporting services | PostgreSQL, Valkey |
| Custom applications | Privately maintained web applications and supporting services |

Some applications use multiple containers. Therefore, the number of containers does not represent the number of independent applications.

Historical services that are no longer present in the current inventory are not presented as active deployments.

### 3.3. Docker Container Evidence

The following screenshot shows selected running containers and their observed states.

![HuHServ2 Docker Containers](../screenshots/huhserv2-docker-containers.png)

The screenshot focuses on infrastructure components and excludes selected privately named application workloads.

---

## 4. Homepage — Self-Hosted Infrastructure Dashboard

Homepage is deployed on HuHServ2 as a centralized dashboard for accessing and monitoring self-hosted applications across the homelab.

It provides a single interface for navigation, service organization, and monitoring information.

### 4.1. Dashboard Features

The configured interface includes:

- Monitoring widgets for HuHServer and HuHServ2.
- Application groups organized by purpose.
- Docker and server administration shortcuts.
- File synchronization and cloud services.
- Media applications.
- Development tools.
- Service status indicators.
- Search, date, and time widgets.

### 4.2. Custom Glassmorphism Interface

The dashboard has been customized using Homepage configuration settings and additional CSS.

The interface uses a personalized **Graphite Glass** appearance.

Implemented design elements include:

- Dark graphite service cards.
- Translucent backgrounds.
- Background blur.
- Thin borders and rounded corners.
- Hover effects.
- Custom information-widget styling.
- A personalized background image.
- Improved text contrast and readability.

The design was configured through `settings.yaml` and `custom.css`.

Customization was performed without replacing the original application or changing its core functionality.

### 4.3. Homepage Screenshots

**Infrastructure Monitoring and Main Services**

![HuHServ2 Homepage Overview](../screenshots/huhserv2-homepage-overview.png)

**Additional Services and Navigation**

![HuHServ2 Homepage Services](../screenshots/huhserv2-homepage-services.png)

These screenshots demonstrate practical experience in customizing and organizing a self-hosted infrastructure dashboard.

---

## 5. Featured Deployment — Penpot

Penpot 2.17 is deployed on HuHServ2 as a self-hosted, multi-container application for UI design and prototyping.

The deployment includes seven Docker containers:

| Component | Container image |
|---|---|
| Frontend | `penpotapp/frontend:2.17` |
| Backend | `penpotapp/backend:2.17` |
| PostgreSQL | `postgres:15` |
| Valkey | `valkey/valkey:8.1` |
| Exporter | `penpotapp/exporter:2.17` |
| MCP | `penpotapp/mcp:2.17` |
| Mailcatcher | `sj26/mailcatcher:latest` |

### 5.1. Networking

All seven containers share the Docker bridge network:

`penpot_penpot`

The inspected network uses the `bridge` driver and is not marked as an internal Docker network.

PostgreSQL and Valkey do not have host-published ports in the observed configuration.

### 5.2. Remote Access

Penpot is accessed privately through Tailscale Serve.

The verified access path is:

**Windows laptop → Tailscale Serve HTTPS :9001 → HuHServ2 loopback :9001 → Penpot Frontend :8080**

The Penpot endpoint was marked `tailnet only` during inspection and was not published through Tailscale Funnel.

Other applications on the server may use separate Serve or Funnel routes.

### 5.3. Technical Case Study

The Penpot deployment has its own detailed documentation, including:

- Container infrastructure and image versions.
- Docker networking and port mappings.
- Private remote access configuration.
- Mermaid architecture diagram.
- Verification procedures.
- Application screenshot.
- Security considerations and limitations.

**[Read the Penpot Self-Hosted Deployment Case Study](penpot-deployment.md)**

*Penpot deployment verification: October 21, 2026.*

---

## 6. Other Self-Hosted Applications

HuHServ2 hosts several additional applications that support everyday workflows and infrastructure experimentation.

### 6.1. Navidrome and Feishin

Navidrome provides a self-hosted music library and streaming interface.

Feishin is used as a music client.

This deployment demonstrates practical experience hosting media applications through Docker.

A detailed technical case study is planned.

### 6.2. Game Dev Hub

Game Dev Hub is a self-hosted application for organizing game development resources and workflows.

The inspected deployment includes:

- Main application container.
- Background news worker.
- Tailscale container used for connectivity.

The application and its worker use a locally built image.

Further documentation will describe the deployment architecture and verified connectivity configuration.

### 6.3. File Sharing and Downloads

Additional services include:

- PairDrop for transferring files between devices.
- ClipCascade for clipboard synchronization.
- MeTube for downloading video and audio.
- qBittorrent for download management.
- Telegram Downloader for automated downloads.

These applications are maintained as part of the personal homelab.

---

## 7. Networking and Remote Connectivity

Tailscale is used to provide connectivity between selected devices and servers.

The infrastructure includes Windows devices, Linux servers, and mobile clients connected through a private Tailnet.

### 7.1. Tailscale Serve

Selected self-hosted applications are exposed to authorized Tailnet clients through Tailscale Serve.

This allows applications bound to the server's loopback interface to be accessed remotely through configured HTTPS endpoints.

### 7.2. Tailscale Funnel

Some separately configured services use Tailscale Funnel for public HTTPS access.

Funnel endpoints must be distinguished from private Tailnet-only services.

The Penpot frontend was verified as a Tailnet-only service.

### 7.3. Connectivity Limitations

The October 21 inspection also reported a Tailscale DNS configuration warning involving `resolvconf`.

The warning was not resolved as part of this portfolio documentation.

Functional remote access to the Penpot interface was observed, but this does not constitute a comprehensive network reliability or security test.

---

## 8. Administration Experience Demonstrated

The HuHServ2 infrastructure demonstrates practical experience with:

### Linux Administration

- Operating Debian on physical hardware.
- Inspecting system information and resource utilization.
- Managing storage and network services.
- Using SSH and command-line administration tools.

### Docker Administration

- Inspecting running containers.
- Identifying container images and versions.
- Reviewing healthcheck status.
- Working with multi-container applications.
- Inspecting Docker networks and port mappings.
- Managing locally built application images.

### Networking

- Configuring private remote access.
- Inspecting Tailscale Serve and Funnel configuration.
- Understanding loopback-bound services and Docker port publishing.
- Distinguishing internal container networking from external access.

### Documentation and Troubleshooting

- Collecting technical evidence using read-only commands.
- Documenting real infrastructure configurations.
- Building Mermaid architecture diagrams.
- Recording observed system behavior and limitations.
- Organizing technical projects using Git and GitHub.

These skills were developed through practical work on a personal homelab rather than claimed commercial customer engagements.

---

## 9. Evidence and Verification

The infrastructure was inspected using command-line tools and administrative interfaces.

Representative read-only commands include:

```bash
cat /etc/os-release
uname -r
lscpu
free -h
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
docker --version
docker ps
docker network ls
docker network inspect penpot_penpot
tailscale status
tailscale serve status
tailscale funnel status
```

Docker container inspection and network membership checks were performed as part of the Penpot deployment investigation.

### 9.1. Verification Results

| Verification | Result |
|---|---|
| Linux system inventory | Recorded |
| Docker workload inventory | Recorded |
| Running container states | Inspected |
| PostgreSQL healthcheck | Healthy |
| Valkey healthcheck | Healthy |
| Penpot Docker network | Verified |
| Penpot host port mappings | Verified |
| Tailscale Serve route for Penpot | Verified |
| Penpot project dashboard | Opened successfully |
| Homepage interface customization | Visually verified |

These checks document the observed environment at the stated dates. They are not a complete security audit, performance benchmark, or uptime guarantee.

---

## 10. Planned Improvements

The following tasks are planned for future development and documentation:

- [x] Document the physical server configuration.
- [x] Record the Docker container inventory.
- [x] Capture screenshots of infrastructure and selected applications.
- [x] Customize the Homepage interface.
- [x] Document the Penpot multi-container deployment.
- [x] Verify the private Tailscale Serve route for Penpot.
- [ ] Document the Navidrome and Feishin deployment.
- [ ] Document the Game Dev Hub architecture.
- [ ] Document a sanitized Docker Compose configuration.
- [ ] Measure resource consumption under normal workloads.
- [ ] Verify OpenMediaVault installation details.
- [ ] Test and document backup and recovery procedures.
- [ ] Investigate outstanding Tailscale DNS warnings.
- [ ] Record additional real troubleshooting cases.
- [ ] Create a complete architecture overview of both physical servers.

---

## 11. Security and Disclosure

This repository documents a personal infrastructure environment.

Credentials, access tokens, private keys, and complete internal network configurations are not intentionally included in the technical descriptions.

Published screenshots and diagrams should be reviewed for identifying project information, private addresses, hostnames, and other sensitive details.

The presence of configured VPN services, published ports, or running containers is not evidence of a completed penetration test or comprehensive security assessment.

Information is provided for educational purposes and to demonstrate practical Linux server administration experience.

---

## Maintainer

**Vyacheslav Sadovsky (MrHuH)**

Linux Server Administration | Debian | Docker | Self-Hosting | Networking

**GitHub:** [sadovskijslava](https://github.com/sadovskijslava)

**Repository:** [homelab-infrastructure](https://github.com/sadovskijslava/homelab-infrastructure)

*Personal homelab project — not a commercial client deployment.*