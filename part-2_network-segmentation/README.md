# Part 2 — Network Segmentation & Firewall Policy

## Description

Part 2 focuses on implementing true zone-based segmentation inside the SOC-in-a-Box Homelab by separating lab networks into dedicated firewall interfaces and enforcing controlled reachability with OPNsense firewall policy.

This phase validates that routing, DHCP, and reachability behave correctly per segment, and that lateral movement paths can be blocked and logged for future SIEM ingestion.

---

## Architecture Goal (Zone-Based Segmentation)

Instead of relying on hypervisor VLAN tagging, segmentation is implemented using one OPNsense interface per zone:

- WAN (upstream)
- LAN (admin / operator network)
- CYBER_RANGE (vulnerable targets)
- AD_LAB (Windows domain segment - staged for later)

This approach creates clear security boundaries, simplifies troubleshooting, and mirrors enterprise firewall zone design.

<details>
<summary>Show VirtualBox screenshots (OPNsense adapters)</summary>

![VirtualBox OPNsense adapters for zone-based segmentation](assets/vbox_opnsense_adapters_part2.png)

</details>

---

## Segments & Addressing

| Segment      | Purpose                          | Subnet            | Gateway         |
|-------------|-----------------------------------|-------------------|-----------------|
| WAN         | Upstream / internet               | 192.168.10.0/24   | (DHCP upstream) |
| LAN         | Admin / Operator (Kali)           | 192.168.20.0/24   | 192.168.20.1    |
| CYBER_RANGE | Vulnerable targets (Chronos, MSF) | 192.168.30.0/24   | 192.168.30.1    |
| AD_LAB      | AD + Windows endpoints (planned)  | 192.168.40.0/24   | 192.168.40.1    |

---

## Objectives

- Build segmentation strategy across internal lab zones
- Assign OPNsense interfaces for LAN / CYBER_RANGE / AD_LAB
- Configure DHCP per segment
- Connect VirtualBox VMs to the correct Internal Networks
- Enforce inter-zone access control (especially lateral movement blocking)
- Validate routing, DNS, and controlled reachability

---

## Environment Summary

### Firewall / Router
- OPNsense (virtualized)
- Interfaces:
  - WAN: em0
  - LAN: em1 (192.168.20.1/24)
  - CYBER_RANGE: em2 (192.168.30.1/24)
  - AD_LAB: em3 (192.168.40.1/24)

<details>
<summary>Show OPNsense screenshots (Interface Assignments)</summary>

![OPNsense interface assignments (WAN/LAN/CYBER_RANGE/AD_LAB)](assets/opnsense_interface_assignments.png)

</details>

### VirtualBox Networks (Internal Networks)
- LAN: `LAB_LAN` (admin/operator zone)
- CYBER_RANGE: `CYBER_RANGE` (vulnerable zone)
- AD_LAB: `AD_LAB` (Windows zone, staged)

### VMs
- Kali (LAN) — operator / attack simulation
- Chronos (CYBER_RANGE) — vulnerable target
- Metasploitable (CYBER_RANGE) — vulnerable target

---

## Implementation Steps

### 1) Create Dedicated Firewall Interfaces (Zone Model)
- Added additional VirtualBox adapters to OPNsense (one per zone)
- Assigned interfaces in OPNsense:
  - LAN → em1
  - CYBER_RANGE → em2
  - AD_LAB → em3
- Set static gateways per zone

### 2) Enable DHCP Per Zone
- Enabled DHCP on LAN to support Kali/operator provisioning
- Enabled DHCP on CYBER_RANGE to support vulnerable target provisioning
- (AD_LAB DHCP staged for later depending on AD design)

### 3) Connect VMs to the Correct Zone
- Kali connected to `LAB_LAN` (LAN zone)
- Chronos connected to `CYBER_RANGE` (CYBER_RANGE zone)
- Metasploitable connected to `CYBER_RANGE` (CYBER_RANGE zone)

### 4) Validate IP Leasing & Reachability
Confirmed leases and segmentation behavior:

- Kali: 192.168.20.199 (LAN)
- Chronos: 192.168.30.114 (CYBER_RANGE)
- Metasploitable: 192.168.30.126 (CYBER_RANGE)

<details>
<summary>Show OPNsense screenshots (DHCPv4 leases)</summary>

![DHCPv4 leases showing LAN + CYBER_RANGE clients](assets/opnsense_dhcp_leases.png)

</details>

---

## Firewall Policy (Key Deliverable)

### CYBER_RANGE Rules (Minimum Viable Secure Policy)

1) Allow DHCP to firewall  
- CYBER_RANGE net → This Firewall (UDP/67)

2) Allow ping to firewall (optional but useful for testing)  
- CYBER_RANGE net → This Firewall (ICMP)

3) Block lateral movement to LAN (LOG ENABLED) ✅  
- CYBER_RANGE net → LAN net (Block + Log)

4) Allow CYBER_RANGE outbound  
- CYBER_RANGE net → any (used for initial validation; can be restricted later to DNS/HTTP/HTTPS)

> Important note: Rule order matters. The lateral movement block must sit ABOVE any broad “allow outbound” rule, or it will never trigger.

<details>
<summary>Show OPNsense screenshots (CYBER_RANGE firewall rules)</summary>

![CYBER_RANGE rules with lateral block + logging](assets/opnsense_cyber_range_rules.png)

</details>

---

## Troubleshooting Notes (Real-World Issues Encountered)

- Kali received a 169.254.x.x address (DHCP failure)
  - Root cause: LAN DHCP service needed to be restored/enabled after segmentation changes
  - Fix: re-enabled / reset DHCPv4 on LAN

- CYBER_RANGE targets initially could not reach the firewall IP (192.168.30.1)
  - Root cause: missing explicit rule allowing CYBER_RANGE to talk to “This Firewall”
  - Fix: added minimal rules (DHCP + ICMP to firewall)

- “Block lateral” rule did not work at first
  - Root cause: it was placed BELOW a broad allow rule (first-match firewall behavior)
  - Fix: moved block rule above allow rule and enabled logging

---

## Validation Checklist

- [x] Kali receives 192.168.20.x from LAN DHCP  
- [x] CYBER_RANGE targets receive 192.168.30.x from CYBER_RANGE DHCP  
- [x] Kali can ping CYBER_RANGE gateway (192.168.30.1)  
- [x] CYBER_RANGE cannot reach LAN once block rule is enabled  
- [x] Block events appear in OPNsense Live View logs  
- [x] Segmentation is stable and repeatable across reboots

---

## Learning Outcomes

- Implemented zone-based segmentation using firewall interfaces (enterprise model)
- Reinforced first-match firewall rule behavior and the importance of rule order
- Distinguished Layer 2 wiring issues vs Layer 3 policy issues during troubleshooting
- Produced clean, loggable lateral-movement denial events for future SIEM ingestion

---

## Next (Part 3)

Part 3 will focus on generating SOC telemetry and pushing it into centralized monitoring:

- Deploy SIEM tooling (Wazuh planned)
- Ingest OPNsense firewall logs
- Add endpoint agents (Linux + Windows)
- Simulate attacks from Kali → CYBER_RANGE and validate detections
- Begin tuning alerts + building investigation workflow
