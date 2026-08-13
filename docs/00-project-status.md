# Proxmox SOC Homelab — Project Status

**Last Updated:** August 13, 2026
**Current Phase:** Phase 1 — Requirements & Architecture
**Project Status:** Architecture planning in progress; deployment has not started.

---

## Project Goal

Build a portfolio-quality cybersecurity homelab on Proxmox VE designed primarily for SOC Analyst and Blue Team roles.

The environment will demonstrate:

* Infrastructure planning and architecture
* Proxmox virtualization
* Network segmentation
* Firewalling and routing
* Windows Server and Active Directory
* Windows endpoint administration
* Linux administration
* Centralized security logging
* SIEM deployment and management
* Endpoint telemetry
* Network IDS
* Detection engineering
* MITRE ATT&CK mapping
* Controlled attack simulation
* SOC-style incident investigation
* Security validation and hardening
* Professional technical documentation

---

## Current Hardware

### Hypervisor

* Dell OptiPlex 7010
* RAM: 32 GB
* GPU: NVIDIA GeForce GTX 1060 3GB
* Proxmox VE 9.2.x

### Primary Storage

**Samsung 870 EVO 1TB**

* Device: `/dev/sda`
* Proxmox installed on this drive
* `local-lvm` used primarily for VM and LXC virtual disks

### Secondary Storage

**Alertseal 120GB SSD**

* Device: `/dev/sdb`
* Partition: `/dev/sdb1`
* Filesystem: ext4
* Mount point: `/mnt/pve-storage`
* Proxmox storage ID: `secondary-storage`

Purpose:

* ISO images
* LXC templates
* Selected backups

---

## Current Proxmox Storage

| Storage             | Type      | Primary Purpose                         |
| ------------------- | --------- | --------------------------------------- |
| `local`             | Directory | ISO, templates, imports, backups        |
| `local-lvm`         | LVM-Thin  | VM/LXC virtual disks                    |
| `secondary-storage` | Directory | ISO images, templates, selected backups |

---

## Completed

* Proxmox VE installed
* Proxmox fully updated
* Host networking operational
* 32 GB RAM installed
* Primary Samsung 870 EVO configured
* Secondary 120GB SSD prepared and validated
* Secondary SSD partitioned using GPT
* Secondary SSD formatted as ext4
* SMART testing completed
* Persistent `/etc/fstab` mount configured
* `/mnt/pve-storage` validated
* `secondary-storage` added to Proxmox
* Initial SOC homelab architecture proposed
* GitHub repository created
* ChatGPT project created for ongoing project work

---

## Current Architecture Direction

### Management / WAN

`vmbr0`

Existing home network used for:

* Proxmox management
* OPNsense WAN connectivity

The Proxmox management plane will remain outside the attackable cybersecurity lab networks.

### Proposed Lab Networks

| Bridge  | Security Zone | Subnet          |
| ------- | ------------- | --------------- |
| `vmbr1` | SOC           | `10.10.10.0/24` |
| `vmbr2` | Corporate     | `10.10.20.0/24` |
| `vmbr3` | DMZ           | `10.10.30.0/24` |
| `vmbr4` | Attack        | `10.10.40.0/24` |

These bridges have **not yet been created**.

---

## Proposed Core Systems

| Hostname   | Role                                | vCPU |  RAM |     Disk |
| ---------- | ----------------------------------- | ---: | ---: | -------: |
| `FW01`     | OPNsense firewall/router + Suricata |    2 | 4 GB |    20 GB |
| `SIEM01`   | Wazuh all-in-one                    |    4 | 8 GB |    80 GB |
| `DC01`     | Windows Server / AD DS / DNS / GPO  |    2 | 4 GB |    60 GB |
| `WIN11-01` | Windows domain workstation          |    2 | 4 GB | 64–80 GB |
| `WEB01`    | Ubuntu DMZ server                   |    2 | 2 GB |    40 GB |
| `KALI01`   | Controlled attack workstation       |    2 | 4 GB | 50–60 GB |

These systems have **not yet been deployed**.

---

## Technology Decisions

### Selected

* Proxmox VE
* OPNsense
* Wazuh
* Suricata
* Sysmon
* Windows Server Active Directory
* Windows 11
* Ubuntu Server
* Kali Linux
* MITRE ATT&CK

### Deferred / Optional

* Splunk

Splunk may be used later for comparison or a standalone detection exercise rather than operating as a second permanent SIEM.

### Excluded From Initial Architecture

* Security Onion
* Separate Elastic Stack
* Separate Suricata sensor VM
* Multiple domain controllers
* Ceph
* Kubernetes
* Multiple firewall appliances
* OpenVAS/GVM as an always-on VM
* GPU passthrough

---

## Architecture Decisions

### ADR-001 — OPNsense Firewall

OPNsense will provide routing and firewall enforcement between cybersecurity lab networks.

### ADR-002 — Separate Proxmox Bridges

Separate internal Proxmox Linux bridges will initially be used instead of VLAN trunking because the lab exists entirely within one hypervisor.

### ADR-003 — Wazuh Primary SIEM

Wazuh will serve as the primary security monitoring and SIEM platform.

### ADR-004 — Security Onion Excluded

Security Onion is excluded from the initial architecture because its resource requirements are not appropriate for a 32 GB single-host environment running the complete enterprise simulation.

### ADR-005 — Suricata on OPNsense

Suricata will initially operate through OPNsense rather than requiring a separate IDS VM.

### ADR-006 — Management Plane Isolation

The Proxmox management interface will remain outside the attackable cybersecurity lab networks.

### ADR-007 — Primary VM Storage

VM and LXC virtual disks will primarily reside on `local-lvm` on the Samsung 870 EVO.

### ADR-008 — Secondary Storage

The 120GB secondary SSD will primarily store ISO images, LXC templates, and selected backups.

---

## Current Phase

### Phase 1 — Requirements & Architecture

Current work:

* Define lab objectives
* Define functional requirements
* Define security requirements
* Confirm infrastructure constraints
* Finalize network zones
* Finalize subnet and IP addressing
* Finalize hostname conventions
* Validate VM inventory
* Validate resource allocations
* Define firewall trust relationships
* Define logging/security data flows
* Record architecture assumptions
* Record architecture decisions
* Finalize logical build sequence

---

## Not Yet Built

* `vmbr1`
* `vmbr2`
* `vmbr3`
* `vmbr4`
* OPNsense
* Active Directory
* Windows 11 endpoint
* Ubuntu DMZ server
* Wazuh
* Suricata configuration
* Sysmon deployment
* Kali attack workstation
* Detection rules
* Attack simulations
* Incident investigations

---

## Next Action

Complete and formally approve **Phase 1 Requirements & Architecture** before making any additional changes to the Proxmox host.

No VMs or additional virtual bridges should be created until the design has been finalized.

---

## Do Not Redo

The following work is already complete and should not be repeated unless a verified problem occurs:

* Proxmox installation
* Proxmox host updates
* GPU/display troubleshooting
* Secondary SSD wipe and preparation
* GPT partition creation
* ext4 filesystem creation
* `/etc/fstab` configuration
* `secondary-storage` Proxmox configuration

Historical troubleshooting should not be included in the portfolio documentation unless it provides a meaningful architectural or security lesson.
