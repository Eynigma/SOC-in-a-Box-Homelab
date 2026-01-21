# SOC-in-a-Box Homelab

## Description

The **SOC-in-a-Box Homelab** is a fully virtualized, two-system cybersecurity environment designed to simulate real-world **Security Operations Center (SOC)** operations.

This project demonstrates the design, deployment, and operation of a complete **Blue Team monitoring infrastructure** using **Proxmox**, **OPNsense**, **Wazuh**, and multiple virtual machines that generate realistic network and security events.

The lab is distributed across two physical systems:

- **Proxmox Server** – Hosts the core SOC infrastructure, including OPNsense (firewall/router) and Wazuh (SIEM) within isolated networks.
- **Main Homelab Workstation** – Runs the operational environment consisting of Windows, Linux, and vulnerable machines to simulate user activity, attacks, and alerts.

---

## Architecture Overview

- **Firewall / Router:** OPNsense (virtualized on Proxmox)
- **SIEM:** Wazuh (manager, indexer, and dashboard)
- **Endpoints & Attack VMs:** Kali Linux, Metasploitable 2, Chronos 1
- **Windows Environment:** Windows Server 2022 (Active Directory), Windows 10 Client 1 & 2 (domain joined)
- **Network Monitoring & Analysis:** Zeek, Sysmon, Wireshark

---

## Project Objectives

- Deploy OPNsense within Proxmox to segment and route lab networks
- Integrate all endpoints with Wazuh for centralized log collection and alerting
- Simulate attack traffic using Kali, Metasploitable, and Chronos 1
- Observe detections, tune rules, and document alert workflows
- Visualize security telemetry through Wazuh dashboards and custom visualizations

---

## Environment Summary

### System 1 — Proxmox Host
- **OPNsense** – Virtualized firewall and router
- **Wazuh SIEM** – Manager, Indexer, and Dashboard

### System 2 — Main Homelab Workstation
- **Kali Linux** – Attack simulation and testing
- **Metasploitable 2** – Vulnerable target for exploit development
- **Chronos 1** – Additional intentionally vulnerable system
- **Windows Server 2022** – Active Directory, DNS, DHCP
- **Windows 10 Client 1** – Domain-joined workstation
- **Windows 10 Client 2** – Secondary workstation for activity simulation

---

## Technologies Used

- **Virtualization:** Proxmox VE
- **Firewall & Routing:** OPNsense
- **SIEM:** Wazuh (Manager, Indexer, Dashboard)
- **Analysis Tools:** Zeek, Wireshark, Sysmon, Sigma Rules
- **Operating Systems:**  
  Windows Server 2022, Windows 10, Kali Linux, Metasploitable 2, Chronos 1

---

## Project Breakdown

1. **[Part 1 – Infrastructure Setup](part-1_infrastructure-setup/)**  
   Configure Proxmox, deploy OPNsense and Wazuh, and establish internal networks.

2. **Part 2 – Endpoint & Domain Configuration**  
   Build a Windows Active Directory domain, connect clients, and configure vulnerable systems.

3. **Part 3 – Attack Simulation & Detection**  
   Simulate adversarial activity, monitor Wazuh alerts, and analyze security logs.

4. **Part 4 – Visualization & Reporting**  
   Create dashboards, correlate alerts, and document incident response workflows.

---

## Future Enhancements

- Integrate Elastic Stack or Graylog for multi-SIEM comparison
- Add Security Onion as a passive NIDS sensor
- Implement TheHive/Cortex for incident ticketing and enrichment
- Map detections to MITRE ATT&CK techniques
- Automate log forwarding using Winlogbeat and Filebeat

---

## Learning Outcomes

- Design and secure virtual SOC infrastructure using Proxmox and OPNsense
- Deploy and manage a Wazuh SIEM for centralized log analysis
- Simulate adversarial behavior and detect malicious activity in real time
- Build detection dashboards and analyze alert pipelines
- Develop Blue Team reporting and incident documentation skills

---

## Connect

🔗 [LinkedIn](https://www.linkedin.com/in/iangoodman13/)
