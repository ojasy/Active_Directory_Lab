🛡️ Active Directory Cybersecurity Home Lab
<div align="center">
Show Image
Show Image
Show Image
Show Image
Show Image
Show Image
A fully functional cybersecurity home lab simulating a corporate network environment.
Built for hands-on learning in offensive security, defensive monitoring, and incident detection.
View Screenshots • View Walkthrough • View References
</div>

📌 Overview
This project documents the complete build of a virtualized cybersecurity home lab using VMware Workstation Pro. The environment simulates a real corporate network featuring:

🏢 Windows Server 2022 acting as a Domain Controller running Active Directory
💻 Windows 10 Pro as a target employee workstation joined to the domain
☠️ Kali Linux as an attacker machine simulating real-world threats
📊 Ubuntu Server with Splunk as the SIEM collecting and analyzing all logs

The lab covers the full attack and defense lifecycle — from setting up the environment, launching brute force attacks, and detecting them in Splunk using Windows Event IDs and Sysmon telemetry.

💡 Inspired by MyDFIR's Active Directory Project Series and avulman's Active Directory Project


🗺️ Network Diagram
<div align="center">
Show Image
</div>

🖥️ Lab Environment
<div align="center">
🖥️ Machine💿 Operating System🌐 IP Address🎯 RoleADDC01Windows Server 2022192.168.10.7Domain ControllerTARGET-PCWindows 10 Pro192.168.10.100Target WorkstationKALI-ATTACKERKali Linux192.168.10.250Attack MachineSPLUNK-SERVERUbuntu Server 22.04192.168.10.10Splunk SIEM
🌐 Network: VMware VMnet8 NAT — 192.168.10.0/24
</div>

🛠️ Technologies Used
<div align="center">
🔧 Tool📋 PurposeVMware Workstation ProVirtualization platformWindows Server 2022Active Directory Domain ServicesWindows 10 ProTarget workstationKali LinuxAttack simulationUbuntu Server 22.04Splunk SIEM hostSplunk Enterprise 10.2.3Log analysis and SIEMSplunk Universal ForwarderLog shipping from Windows machinesSysmon by SysinternalsEndpoint process and event monitoringCrowbarRDP brute force toolHydraNetwork login crackerAtomic Red TeamMITRE ATT&CK adversary simulationPowerShellScripting and AD automation
</div>

🎯 Skills Demonstrated
✅ Virtualization and VM lifecycle management with VMware
✅ Active Directory domain setup, OU structure, and user management
✅ Static IP addressing, DNS configuration, and network segmentation
✅ SIEM deployment and configuration with Splunk Enterprise
✅ Endpoint telemetry collection using Sysmon and Splunk Universal Forwarder
✅ Offensive security — RDP brute force simulation using Crowbar and Hydra
✅ Defensive security — attack detection using Splunk Event IDs 4624 and 4625
✅ MITRE ATT&CK framework technique simulation with Atomic Red Team
✅ PowerShell scripting for Active Directory user automation
✅ Linux server administration and Netplan network configuration

📁 Repository Structure
📦 active-directory-lab
 ┣ 📂 screenshots
 ┃ ┣ 🖼️ 03-01-server2022-setup.png
 ┃ ┣ 🖼️ 07-01-splunk-dashboard.png
 ┃ ┣ 🖼️ 10-04-ad-users-created.png
 ┃ ┣ 🖼️ 14-01-splunk-failed-logins.png
 ┃ ┗ 🖼️ 15-03-splunk-art-detection.png
 ┣ 📂 diagrams
 ┃ ┗ 🗺️ 01-01-network-diagram.png
 ┣ 📂 scripts
 ┃ ┗ 📜 create-users.ps1
 ┣ 📂 configs
 ┃ ┗ ⚙️ inputs.conf
 ┗ 📄 README.md

🔬 Project Walkthrough
<details>
<summary>📐 Part 1 — Network Planning</summary>
Designed the lab topology using draw.io before building anything. Mapped all four VMs with their static IPs on the 192.168.10.0/24 network. Planning the diagram first saved significant troubleshooting time later.
</details>
<details>
<summary>🔧 Part 2 — VMware Network Configuration</summary>
Configured VMware VMnet8 as a NAT network with subnet 192.168.10.0/24 so all VMs can communicate with each other and access the internet through the host machine.
</details>
<details>
<summary>🖥️ Part 3 — Windows Server 2022 Setup</summary>
Created the Domain Controller VM (ADDC01) with 4GB RAM and 2 vCPUs. Assigned static IP 192.168.10.7, installed VMware Tools, and renamed the machine to ADDC01. Set DNS to 127.0.0.1 (self) in preparation for Active Directory.
</details>
<details>
<summary>💻 Part 4 — Windows 10 Setup</summary>
Created the target workstation VM (TARGET-PC) with static IP 192.168.10.100. Set DNS to point to the domain controller at 192.168.10.7 so it can resolve the domain during the join process.
</details>
<details>
<summary>☠️ Part 5 — Kali Linux Setup</summary>
Created the attacker VM with static IP 192.168.10.250. Configured /etc/network/interfaces for persistent static IP assignment. Verified connectivity to all other VMs.
</details>
<details>
<summary>📊 Part 6 — Ubuntu Server Setup</summary>
Created the Splunk server VM with static IP 192.168.10.10. Configured Netplan YAML for static IP assignment. SSHed into the server from the Windows host for easier administration.
</details>
<details>
<summary>🔍 Part 7 — Splunk Installation and Configuration</summary>
Installed Splunk Enterprise 10.2.3 on Ubuntu Server via .deb package. Configured receiving port 9997 for log ingestion. Created the endpoint index for storing Windows event logs. Accessed the web interface at http://192.168.10.10:8000.
</details>
<details>
<summary>👁️ Part 8 — Sysmon Installation</summary>
Installed Sysmon64 on both Windows machines using the olafhartong sysmon-modular configuration. This provides detailed endpoint telemetry including process creation, network connections, file modifications, and registry changes — far beyond what Windows Event Logs provide by default.
</details>
<details>
<summary>📡 Part 9 — Splunk Universal Forwarder</summary>
Installed Splunk Universal Forwarder on ADDC01 and TARGET-PC. Configured inputs.conf to forward Security, System, Application, and Sysmon/Operational event logs to Splunk at 192.168.10.10:9997. Verified data flow with index=endpoint search returning events from both machines.
</details>
<details>
<summary>🏢 Part 10 — Active Directory Configuration</summary>
Promoted ADDC01 to Domain Controller for demodomain.local using the AD DS role wizard. Created IT and HR Organizational Units. Created domain users jsmith (John Smith) and tsmith (Terry Smith) in the IT OU with Password1234! for lab use.
</details>
<details>
<summary>🔗 Part 11 — Domain Join</summary>
Joined TARGET-PC to demodomain.local by entering domain credentials in System Properties. After restart the login screen showed DEMODOMAIN context. Successfully logged in as domain user jsmith confirming the domain join worked.
</details>
<details>
<summary>🖥️ Part 12 — Remote Desktop Configuration</summary>
Enabled RDP on TARGET-PC via registry (fDenyTSConnections = 0) and opened Windows Firewall port 3389. Added jsmith and tsmith to the Remote Desktop Users group. Disabled Network Level Authentication (UserAuthentication = 0) to allow Crowbar RDP brute force attempts.
</details>
<details>
<summary>⚔️ Part 13 — Brute Force Attack Simulation</summary>
Used Crowbar and Hydra from Kali Linux (192.168.10.250) to simulate an RDP brute force attack against tsmith on TARGET-PC (192.168.10.100:3389). Created passwords.txt wordlist using the first 20 entries from rockyou.txt with the target password appended. Confirmed port 3389 was open using nmap prior to attack.
Attack command:
bashcrowbar -b rdp -u tsmith -C passwords.txt -s 192.168.10.100/32
hydra -t 1 -V -f -l tsmith -P passwords.txt rdp://192.168.10.100
</details>
<details>
<summary>🔎 Part 14 — Attack Analysis in Splunk</summary>
Analyzed the brute force attack in Splunk using SPL queries. Identified flood of EventCode 4625 (failed logon) events followed by EventCode 4624 (successful logon). Used table command to display Account_Name and Source_Network_Address fields confirming the attack origin.
Key SPL queries used:
index=endpoint EventCode=4625
index=endpoint EventCode=4624
index=endpoint EventCode=4625 | stats count by Account_Name
index=endpoint host="TARGET-PC" | table _time, Account_Name, Source_Network_Address
</details>
<details>
<summary>🧪 Part 15 — Atomic Red Team</summary>
Installed Atomic Red Team framework on TARGET-PC. Executed MITRE ATT&CK technique T1136.001 (Create Local Account) to simulate real adversary behavior. Confirmed detection in Splunk with 3,932 events captured from TARGET-PC including the new account creation event.
Test command:
powershellInvoke-AtomicTest T1136.001
</details>

📸 Screenshots
🗺️ Network Diagram
Show Image
🏢 Active Directory — Users and OUs Created
Show Image
📊 Splunk Dashboard
Show Image
⚔️ Brute Force Attack — Failed Logins Detected (EventCode 4625)
Show Image
🔍 Splunk Event Details
Show Image
🧪 Atomic Red Team Events in Splunk — 3,932 Events Captured
Show Image

📖 References
ResourceLinkMyDFIR — Active Directory Project YouTube SeriesWatch Hereavulman — Active Directory Project GitHubView HereAtomic Red Team by Red CanaryView HereSysmon Modular Config by olafhartongView HereSplunk DocumentationView HereMITRE ATT&CK FrameworkView Here

⚠️ Disclaimer

This lab is built entirely for educational purposes in a controlled isolated environment. All attack simulations were performed exclusively against virtual machines I own. Never use these techniques against systems you do not have explicit written permission to test.


<div align="center">
🛡️ Built by Ojas Yajnik — May 2026
Show Image
</div>