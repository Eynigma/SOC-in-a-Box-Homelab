# Part 1 – Infrastructure Setup

## Description

Part 1 establishes the **foundation** of the SOC-in-a-Box Homelab by implementing correct **segmentation, switching behavior, and firewall placement**.

This phase focuses on building a stable base that behaves like a real environment:
- clean WAN/LAN separation
- predictable VLAN behavior (trunk vs access)
- firewall routing and DHCP boundaries
- virtualization settings that persist across reboots

This phase also documents the real troubleshooting steps required to reach a stable design.

---

## Goals

- Establish VLAN-based segmentation on the router and managed switch
- Ensure correct trunk vs access port behavior
- Stand up OPNsense as the perimeter firewall in a virtualized environment
- Validate WAN DHCP and internal LAN addressing
- Confirm persistence (no config loss on reboot)
- Prepare the lab for SIEM ingestion in Part 2

---

## Architecture (Part 1 Scope)

- **Router:** UCG-Ultra (VLANs defined here, details below)
- **Switch:** Managed switch enforcing VLAN trunk/access roles
- **Firewall:** OPNsense in VirtualBox on the Main Homelab Workstation
- **SOC Host:** Proxmox Server reserved for Wazuh and SOC services (Part 2)

---

## VLANs and Networks

> Exact VLAN IDs and subnets are documented here for reproducibility.
> (Main project README stays high-level.)

### VLAN List (Fill-in / Confirmed)
- **VLAN 1 – Home / Management:** `__________________`
- **VLAN 10 – Lab Ingress (Firewall WAN):** `__________________`
- **VLAN 20 – Internal Lab (Firewall LAN):** `__________________`
- **VLAN 30 – Cyber Range / Targets:** `__________________`
- **VLAN 40 – Windows / AD Lab:** `__________________`
- **VLAN 50 – Security / SOC Services:** `__________________`

### DHCP
- VLAN 10 DHCP source: `__________________`
- VLAN 20 DHCP source: `OPNsense` (LAN-side DHCP enabled)
- Additional VLAN DHCP strategy (planned): `__________________`

---

## Switch Port Roles

> This section is intentionally explicit because incorrect uplink tagging was the root cause of early issues.

### Uplink Port to Router (Trunk)
- **Port:** `__________________`
- **VLAN 1:** Untagged (native)
- **VLANs 10/20/30/40/50:** Tagged
- **PVID:** 1

### Workstation Second NIC (Access Port for Lab Ingress)
- **Port:** `__________________`
- **VLAN 10:** Untagged
- **VLAN 1:** Not untagged
- **PVID:** 10

### Notes
- End devices must be untagged on one VLAN (access) unless the endpoint is VLAN-aware
- Tagging VLAN 1 on the router uplink caused VLAN leakage and incorrect DHCP behavior (192.168.1.x symptom)

---

## OPNsense VirtualBox Deployment

### VM Hardware (Required for Stability)
- **Disk Controller:** SATA (AHCI)
- **Disk Size:** 20–32 GB (recommended 32 GB)
- **Why:** IDE controller caused filesystem read-only state and config loss on reboot

### Installation Mode (Important)
- To install to disk from ISO:
  - login: `installer`
  - password: `opnsense`
- Logging in as `root` boots **Live Mode** (no disk install, no persistence)

---

## OPNsense Network Interfaces

### Adapter Layout
- **Adapter 1 (WAN):** Bridged Adapter → Workstation **second NIC** (connected to VLAN 10 access port)
- **Adapter 2 (LAN):** Internal Network → `OPNsense_lan`

### Interface Assignment (OPNsense)
- WAN → `vtnet0`
- LAN → `vtnet1`

### LAN Addressing
- LAN IP: `__________________` (example: 192.168.20.1/24)
- DHCP on LAN: Enabled
- DHCP range: `__________________`

---

## Validation Checklist

### VLAN / DHCP Validation
- Workstation second NIC pulls VLAN 10 addressing correctly (or remains isolated as expected)
- OPNsense WAN receives an IP from VLAN 10 DHCP
- OPNsense LAN is static and does not pull from VLAN 1

### Persistence Validation (Critical)
- Reboot OPNsense VM
- Confirm:
  - Interface assignments remain correct
  - LAN IP remains correct
  - No filesystem read-only behavior
  - Config persists across reboots

### Test VM Validation
- Attach a test VM to `OPNsense_lan`
- Confirm:
  - receives LAN DHCP
  - can ping OPNsense LAN IP
  - routes through firewall as designed

---

## Troubleshooting Highlights (What Was Solved)

### Issue 1: Devices always pulled 192.168.1.x
**Symptoms**
- Workstation second NIC repeatedly received 192.168.1.x
- Firewall WAN received 192.168.1.x

**Root Cause**
- Switch uplink tagging was inverted:
  - VLAN 10 was not being carried correctly
  - VLAN 1 tagging/untagging was incorrect

**Fix**
- Router uplink port configured as:
  - VLAN 1 untagged
  - VLAN 10 tagged
  - correct PVID

---

### Issue 2: OPNsense settings would not persist
**Symptoms**
- Settings reverted after reboot
- Disk showed 100% usage
- Filesystem mounted read-only

**Root Cause**
- VM disk attached to IDE controller
- Also, earlier attempts booted Live Mode (`root`) instead of installer mode (`installer`)

**Fix**
- Rebuilt OPNsense VM using SATA controller
- Installed to disk via `installer` user
- Removed ISO after install and set boot order to Hard Disk

---

## Outcome of Part 1

By the end of Part 1:
- VLAN behavior is predictable and correct
- OPNsense is installed to disk and stable
- WAN/LAN separation is verified
- Internal network is ready for SIEM ingestion and endpoint onboarding (Part 2)

---

## Next: Part 2 – SIEM Deployment & Log Ingestion

Part 2 will deploy **Wazuh on Proxmox** and validate end-to-end ingestion from:
- OPNsense firewall logs
- Linux endpoints
- Windows endpoints (as added in later phases)
