# Private Cloud & Edge Compute Environment

**Status:** Production / Active  
**Primary Hypervisor:** Proxmox VE Cluster  
**Storage Fabric:** TrueNAS Scale (ZFS)  

## 🌐 Overview
This repository documents the architecture, networking topology, and infrastructure-as-code deployments for my personal 42U Edge Compute Datacenter. 

This environment serves as an enterprise-grade R&D sandbox for testing high-availability (HA) clustering, automated data pipelines, network isolation (VLANs), and AI-augmented IoT infrastructure before deploying concepts into production SaaS environments.

---

## 🖥️ Bare Metal & Hardware Topology
* **Compute Nodes:** `[e.g., 3x Supermicro 1U Servers, Dual Xeon, 256GB RAM]`
* **Storage Array:** TrueNAS Scale utilizing `[e.g., ~30TB raw storage via Seagate Ironwolf drives in RAID-Z2]`
* **Networking Gear:** `[e.g., Unifi Dream Machine Pro, 24-Port PoE Switch]`
* **Power Management:** `[e.g., 2x APC Smart-UPS, monitored via SNMP]`

---

## 🕸️ Network Architecture & Security (Zero Trust)
The network is heavily segmented to isolate IoT devices, guest access, and core infrastructure management.
* **VLAN 10 (Management):** Restricted access for Proxmox GUI, TrueNAS, and IPMI/iDRAC.
* **VLAN 20 (Production Services):** Public-facing reverse proxies and core application stacks.
* **VLAN 30 (IoT & Automation):** "Krakoa" Home Assistant instance, Konnected boards, and isolated smart switches. *No outbound internet access permitted without explicit firewall rules.*
* **Ingress/Egress:** Handled via `[e.g., Cloudflare Tunnels / Nginx Proxy Manager]` with strict Geo-IP blocking and fail2ban integration.

---

## 🧠 Core Services & Orchestration
* **Identity & DNS:** `[e.g., Pi-Hole / AdGuard / local DNS mapping]`
* **Automation Hub (Krakoa):** Home Assistant OS managing environmental sensors, security states, and energy monitoring.
* **Data Integration:** n8n instances for webhooks and automated API handshakes between local hardware and cloud services.
* **Monitoring & Telemetry:** `[e.g., Grafana + Prometheus / Uptime Kuma]` tracking thermal states, drive health, and API latency.

---

## ⚙️ Disaster Recovery & Backups
* **Local Redundancy:** Proxmox VM snapshots taken daily and stored on a dedicated ZFS dataset.
* **Offsite Strategy:** Incremental encrypted backups pushed to a remote 1U storage "slab" at a local QTS colocation facility.
