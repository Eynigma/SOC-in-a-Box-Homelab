<h1>SOC-in-a-Box Homelab</h1>

<h2>Description</h2>
<p>
The <b>SOC-in-a-Box Homelab</b> is a fully virtualized, two-system cybersecurity environment designed to simulate real-world <b>Security Operations Center (SOC)</b> functions.
This project demonstrates the process of designing, deploying, and operating a complete Blue-Team monitoring infrastructure using <b>Proxmox</b>, <b>OPNsense</b>, <b>Wazuh</b>, and multiple virtual machines that generate authentic network and security events.
</p>

<p>
The lab is distributed across two physical systems:
</p>

<ul>
  <li><b>Proxmox Server</b> – Hosts the core SOC infrastructure, including OPNsense (firewall/router) and Wazuh (SIEM) within isolated VLANs.</li>
  <li><b>Main Homelab Workstation</b> – Runs the operational environment consisting of Windows, Linux, and vulnerable machines to simulate user activity, attacks, and alerts.</li>
</ul>

<br/>

<h2>Architecture Overview</h2>

<ul>
  <li><b>Firewall / Router:</b> OPNsense (running on Proxmox)</li>
  <li><b>SIEM:</b> Wazuh (manager, indexer, and dashboard)</li>
  <li><b>Endpoints & Attack VMs:</b> Kali Linux, Metasploitable 2, Chronos 1</li>
  <li><b>Windows Environment:</b> Windows Server 2022 (AD), Windows 10 Client 1 & 2 (domain joined)</li>
  <li><b>Network Monitoring:</b> Zeek, Sysmon, and Wireshark for packet and event inspection</li>
</ul>

<br/>

<h2>Project Objectives</h2>

<ul>
  <li>Deploy OPNsense within Proxmox to segment and route lab VLANs</li>
  <li>Integrate all endpoints with Wazuh for centralized log collection and alerting</li>
  <li>Simulate attack traffic using Kali, Metasploitable, and Chronos 1</li>
  <li>Observe detections, tune rules, and document the alerting process</li>
  <li>Visualize data through Wazuh dashboards and custom visualizations</li>
</ul>

<br/>

<h2>Environment Summary</h2>

<ul>
  <li><b>System 1 (Proxmox Host)</b>
    <ul>
      <li>OPNsense – Virtualized firewall/router</li>
      <li>Wazuh SIEM – Manager, Indexer, and Dashboard</li>
    </ul>
  </li>

  <li><b>System 2 (Main Homelab Workstation)</b>
    <ul>
      <li>Kali Linux – Attack simulation and testing</li>
      <li>Metasploitable 2 – Vulnerable target for exploits</li>
      <li>Chronos 1 – Additional vulnerable instance</li>
      <li>Windows Server 2022 – Active Directory, DNS, DHCP</li>
      <li>Windows 10 Client 1 – Domain-joined workstation</li>
      <li>Windows 10 Client 2 – Secondary workstation for activity simulation</li>
    </ul>
  </li>
</ul>

<br/>

<h2>Technologies Used</h2>

<ul>
  <li><b>Virtualization:</b> Proxmox VE</li>
  <li><b>Firewall & Routing:</b> OPNsense</li>
  <li><b>SIEM:</b> Wazuh (Manager, Dashboard, Indexer)</li>
  <li><b>Analysis Tools:</b> Zeek, Wireshark, Sysmon, Sigma Rules</li>
  <li><b>Operating Systems:</b> Windows Server 2022, Windows 10, Kali Linux, Metasploitable 2, Chronos 1</li>
</ul>

<br/>

<h2>Project Breakdown</h2>

<ol>
  <li><b>Part 1 – Infrastructure Setup:</b> Configure Proxmox, deploy OPNsense and Wazuh, establish VLANs.</li>
  <li><b>Part 2 – Endpoint & Domain Configuration:</b> Build Windows AD domain, connect clients, and configure vulnerable VMs.</li>
  <li><b>Part 3 – Attack Simulation & Detection:</b> Simulate attacks, monitor Wazuh alerts, and analyze logs.</li>
  <li><b>Part 4 – Visualization & Reporting:</b> Create dashboards, correlate alerts, and document incidents.</li>
</ol>

<br/>

<h2>Future Enhancements</h2>

<ul>
  <li>Integrate Elastic Stack or Graylog for multi-SIEM comparison</li>
  <li>Add Security Onion as a passive NIDS sensor</li>
  <li>Implement TheHive/Cortex for incident ticketing and enrichment</li>
  <li>Map detections to MITRE ATT&CK techniques</li>
  <li>Automate log forwarding using Winlogbeat and Filebeat</li>
</ul>

<br/>

<h2>Learning Outcomes</h2>

<ul>
  <li>Design and secure virtual SOC infrastructure using Proxmox and OPNsense</li>
  <li>Deploy and manage a Wazuh SIEM for centralized log analysis</li>
  <li>Simulate adversarial activity and detect malicious behavior in real time</li>
  <li>Build detection dashboards and analyze alert pipelines</li>
  <li>Develop Blue Team reporting and incident documentation skills</li>
</ul>

<br/>

<h2>Connect</h2>

<p>
📎 <a href="https://www.linkedin.com/in/iangoodman13/">LinkedIn</a>  
<br/>
</p>
