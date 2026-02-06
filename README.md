# SOC-in-a-Box Homelab

## Description

The **SOC-in-a-Box Homelab** is a segmented, multi-system cybersecurity environment designed to simulate real-world **Security Operations Center (SOC)** workflows with an emphasis on **network security, traffic visibility, and centralized monitoring**.

This project focuses on building a realistic **Blue Team defensive lab** from the ground up, emphasizing proper **network architecture, firewall placement, VLAN segmentation, and log visibility** before moving into detection engineering and incident response.

The lab is intentionally built in phases, with each stage documenting both successful configurations and real-world troubleshooting scenarios encountered during deployment.

The environment is distributed across two physical systems:

- **Firewall & SOC Infrastructure Host** – Runs the virtualized perimeter firewall and SOC monitoring services.
- **Main Homelab Workstation** – Hosts attacker, defender, and vulnerable systems used to generate realistic security telemetry.

---

## Architecture Overview

- **Firewall / Router:** OPNsense (virtualized)
- **SIEM / Log Platform:** Wazuh (planned integration)
- **Endpoints & Attack VMs:** Kali Linux, vulnerable Linux targets
- **Windows Environment:** Windows systems planned for later phases
- **Network Visibility & Analysis:** Firewall logs, host logs, packet analysis

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

### System 1 — Firewall / SOC Infrastructure Host
- **OPNsense Firewall**
  - Virtualized with dual interfaces (WAN + LAN)
  - WAN bridged into a dedicated Lab VLAN
  - LAN providing isolated internal lab networks
- **SIEM Platform (Planned)**
  - Wazuh Manager, Indexer, and Dashboard
  - Will ingest firewall and endpoint telemetry

### System 2 — Main Homelab Workstation
- **Kali Linux**
  - Attack simulation
  - Traffic generation
- **Vulnerable Linux Targets**
  - Exploitation testing
  - Log and alert generation
- **Additional Windows & Linux Endpoints** *(Planned)*

---

## Technologies Used

- **Virtualization:** VirtualBox, Proxmox (planned expansion)
- **Firewall & Routing:** OPNsense
- **Networking:** VLANs, managed switch configuration
- **SIEM:** Wazuh *(integration in progress)*
- **Analysis Tools:** Packet capture, firewall logging
- **Operating Systems:**  
  OPNsense, Kali Linux, Linux targets, Windows (planned)

---

## Project Breakdown

1. **[Part 1 – Infrastructure Setup](part-1_infrastructure-setup/)** *(In Progress)*  
   - VLAN design and segmentation  
   - Managed switch configuration  
   - Virtualized OPNsense firewall deployment  
   - WAN/LAN separation  
   - Hypervisor troubleshooting and persistence fixes  

2. **Part 2 – SIEM Deployment & Log Ingestion** *(Planned)*  
   - Deploy Wazuh  
   - Ingest firewall and endpoint logs  
   - Validate log pipelines  

3. **Part 3 – Endpoint & Attack Simulation** *(Planned)*  
   - Simulate adversarial behavior with Kali  
   - Generate realistic alerts and telemetry  

4. **Part 4 – Detection, Visualization & Reporting** *(Planned)*  
   - Alert tuning  
   - Dashboard creation  
   - SOC-style investigation workflows  

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
