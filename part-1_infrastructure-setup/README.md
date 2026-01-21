# Part 1 — Infrastructure Setup

This section documents the foundational infrastructure required to support the SOC-in-a-Box homelab.
It focuses on network placement, host-level correctness, internal routing, and security boundaries
before introducing firewall, SIEM, or detection tooling.

---

## Phases

### Phase 0 — Proxmox Foundation & Secure Management Access  
Establishes a stable and secure baseline for the Proxmox hypervisor, including correct network placement,
routing, and restricted management access.

➡️ **Documentation:**  
[Phase 0 – Proxmox Foundation & Secure Management Access](phase-0_proxmox-foundation.md)

---

### Phase 1 — Proxmox Bridges & Internal Firewall (Planned)
Design and deploy dedicated Proxmox bridges to support an internal firewall (OPNsense), enabling
controlled routing between lab segments.

---

### Phase 2 — SIEM Infrastructure (Planned)
Deploy the Wazuh SIEM stack within the lab network and integrate log sources from endpoints,
firewall, and network services.

---

## Scope of This Part

- Proxmox host configuration and validation
- Management plane access and isolation
- Internal bridge design
- Firewall and SIEM readiness

This part intentionally avoids endpoint configuration and attack simulation, which are covered in later sections.
