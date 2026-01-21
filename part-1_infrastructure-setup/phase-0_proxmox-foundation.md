# Phase 0 – Proxmox Foundation & Secure Management Access

## Objective
Establish a stable and secure foundation for the SOC-in-a-Box homelab by correctly placing the Proxmox hypervisor inside a dedicated Lab network while maintaining restricted, auditable management access from the Home network.

All subsequent firewall, SIEM, and detection components depend on this baseline being correct.

---

## Network Context

- **Home Network:** 192.168.1.0/24
- **Lab Network:** 192.168.10.0/24
- **Proxmox Management IP:** 192.168.10.8
- **Proxmox Web Interface:** https://192.168.10.8:8006

---

## Initial Issue Encountered

After moving the Proxmox host into the Lab network, the web management interface was unreachable from the Home network despite correct VLAN configuration.

Symptoms included:
- Inability to access the Proxmox UI from Home
- No default route present on the Proxmox host
- `ip route` returned no output

---

## Root Cause Analysis

The issue was traced to an over-complicated host network configuration:

- Multiple manually defined bridges
- No clearly defined management interface
- Missing or ineffective default gateway
- Resulting host-level routing failure

Because the Proxmox host lacked a valid routing table, return traffic to the Home network could not be delivered.

---

## Resolution

The Proxmox networking configuration was simplified to a single management bridge:

- **Bridge:** vmbr0
- **Physical NIC:** eno1
- **IP Address:** 192.168.10.8/24
- **Gateway:** 192.168.10.1

Routing was restored and validated using `ip route`, and Proxmox management access from the Home network was confirmed.

Temporary broad firewall rules used during troubleshooting were removed to reestablish strict network segmentation.

---

## Security Rationale

- Hypervisors and security tooling reside in restricted network segments, not user LANs.
- Management access is explicit, controlled, and auditable.
- Host-level correctness is required before deploying firewalls or SIEMs.

This mirrors real-world SOC and enterprise infrastructure design principles.

---

## Checkpoint

A host configuration checkpoint was created after validating routing and access.  
This checkpoint serves as a rollback-safe baseline before introducing Proxmox bridges, OPNsense, and the SIEM stack.

