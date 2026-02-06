# SOC-in-a-Box Homelab

## Description

The **SOC-in-a-Box Homelab** is a segmented, multi-system cybersecurity environment designed to simulate real-world **Security Operations Center (SOC)** workflows with an emphasis on **network security, traffic visibility, and centralized monitoring**.

This project focuses on building a realistic **Blue Team defensive lab** from the ground up, emphasizing proper **network architecture, firewall placement, VLAN segmentation, and log visibility** before moving into detection engineering and incident response.

The lab is intentionally built in phases, with each stage documenting both successful configurations and real-world troubleshooting scenarios encountered during deployment.

The environment is distributed across two physical systems:

- **Proxmox Server** – Hosts core SOC infrastructure services (SIEM and security tooling) on isolated networks.
- **Main Homelab Workstation** – Runs the operational environment (attack and target VMs) and the perimeter firewall VM to simplify connectivity while maintaining segmentation.

---

## Architecture Overview

- **Firewall / Router:** OPNsense (virtualized on the Main Homelab Workstation in VirtualBox)
- **SIEM:** Wazuh (planned on Proxmox: manager, indexer, and dashboard)
- **Endpoints & Attack VMs:** Kali Linux, vulnerable Linux targets (additional endpoints added in later phases)
- **Windows Environment:** Windows systems planned for later phases (AD + clients)
- **Network Visibility & Analysis:** Firewall logs, host telemetry, packet analysis

---

## Project Objectives

- Design and implement proper VLAN segmentation for a SOC lab
- Deploy OPNsense as a virtualized perimeter firewall
- Establish correct WAN/LAN separation and routing
- Route lab traffic through the firewall for inspection and logging
- Prepare the environment for centralized SIEM ingestion
- Document real-world troubleshooting across networking, virtualization, and OS layers

---

## Environment Summary

### System 1 — Proxmox Host (SOC Services)
- **Wazuh SIEM** *(Planned for Part 3)*  
  Manager, Indexer, and Dashboard hosted on Proxmox to centralize visibility across the lab.
- **SOC Services Expansion** *(Planned)*  
  Additional security tooling and dashboards as the lab grows.

### System 2 — Main Homelab Workstation (Operational + Perimeter)
- **OPNsense Firewall** *(Active / In Use)*  
  Virtualized in VirtualBox with dual interfaces:
  - WAN bridged into the Lab ingress VLAN
  - LAN isolated as an internal network for lab routing and segmentation
- **Kali Linux**  
  Attack simulation and traffic generation.
- **Vulnerable Linux Targets**  
  Exploitation testing, log and alert generation.
- **Additional Windows & Linux Endpoints** *(Planned)*  
  Windows Server + Windows clients added in later phases.


---

## Technologies Used

- **Virtualization:** VirtualBox, Proxmox VE
- **Firewall & Routing:** OPNsense
- **Networking:** VLANs, managed switch configuration
- **SIEM:** Wazuh (Manager, Indexer, Dashboard)
- **Analysis Tools:** Packet capture, firewall logging
- **Operating Systems:** OPNsense, Kali Linux, Linux targets, Windows

---

## Project Breakdown

1. **[Part 1 – Infrastructure Setup](part-1_infrastructure-setup/)** *(In Progress)*
   - VLAN design and segmentation (foundation)
   - Managed switch configuration (trunk vs access)
   - OPNsense VirtualBox deployment (WAN/LAN separation)
   - Hypervisor troubleshooting and persistence fixes

2. **Part 2 – Network Segmentation & Firewall Policy** *(Planned)*
   - Build out segmentation strategy across internal lab VLANs
   - Configure OPNsense firewall rules (inter-VLAN access control)
   - Connect VirtualBox VMs to the correct VLANs/networks
   - Validate routing, DNS, and controlled reachability per segment

3. **Part 3 – Wazuh Deployment & Log Ingestion** *(Planned)*
   - Deploy Wazuh on Proxmox (manager, indexer, dashboard)
   - Ingest firewall logs and endpoint telemetry
   - Validate log pipelines and baseline dashboards

4. **Part 4 – Endpoint & Domain Configuration** *(Planned)*
   - Build Windows AD environment
   - Domain-join clients and enable endpoint logging (Sysmon, auditing)
   - Expand telemetry sources

5. **Part 5 – Attack Simulation, Detection & Reporting** *(Planned)*
   - Simulate adversarial activity
   - Monitor alerts, tune detections, and map to MITRE ATT&CK
   - Document SOC triage workflows, dashboards, and reporting
  
---

## Future Enhancements

- Full Wazuh SIEM deployment
- Windows Active Directory lab
- Additional segmented VLANs (Red / Blue separation)
- Detection mapping to MITRE ATT&CK
- Ticketing and incident tracking integration
- Automation for log ingestion and enrichment

---

## Learning Outcomes

- Design and troubleshoot VLAN-based network segmentation
- Deploy and manage a virtualized firewall in a SOC context
- Understand real-world WAN/LAN routing behavior
- Diagnose hypervisor and storage-layer issues affecting security infrastructure
- Build a foundation for SIEM-based detection and monitoring

---

## Connect

🔗 [LinkedIn](https://www.linkedin.com/in/iangoodman13/)
