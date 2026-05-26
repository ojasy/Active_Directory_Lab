\# 🛡️ Active Directory Cybersecurity Home Lab



> Complete home lab simulating a corporate network with Active Directory, Splunk SIEM, Kali Linux brute force attack simulation, and Atomic Red Team adversary emulation.



!\[VMware](https://img.shields.io/badge/VMware-Workstation%20Pro-blue) !\[Splunk](https://img.shields.io/badge/Splunk-SIEM-orange) !\[ActiveDirectory](https://img.shields.io/badge/Active%20Directory-Windows%20Server%202022-0078D4) !\[Kali](https://img.shields.io/badge/Kali%20Linux-Attacker-557C94) !\[MITRE](https://img.shields.io/badge/MITRE-ATT%26CK-red)



\---



\## 📋 Project Overview



This lab simulates a real-world corporate network environment where an RDP brute force attack against a domain-joined workstation is launched from Kali Linux, detected in Splunk SIEM, and analyzed using Windows Event IDs and Sysmon telemetry. Atomic Red Team is used to simulate additional MITRE ATT\&CK techniques and confirm detection capability.



\*\*Attack simulated:\*\* RDP Brute Force using Crowbar and Hydra (MITRE ATT\&CK T1110)



\*\*Full pipeline:\*\*

Windows 10 Endpoint

↓ (Sysmon logs)

Splunk Universal Forwarder

↓ (Port 9997)

Splunk SIEM

↓ (EventCode 4625 / 4624 fires)

SOC Analyst Investigation



\---



\## 🏗️ Architecture



| Component | Role | IP |

|---|---|---|

| ADDC01 | Domain Controller — Windows Server 2022 | 192.168.10.7 |

| TARGET-PC | Target Workstation — Windows 10 Pro | 192.168.10.100 |

| KALI-ATTACKER | Attack Machine — Kali Linux | 192.168.10.250 |

| SPLUNK-SERVER | SIEM — Ubuntu Server 22.04 | 192.168.10.10 |



All VMs run on VMware Workstation using VMnet8 NAT networking — fully isolated from external networks.



\---



\## 🗺️ Network Architecture Diagram

```mermaid

flowchart TD

&#x20;   subgraph KALI\["☠️ KALI LINUX - 192.168.10.250"]

&#x20;       A1\[👤 Attacker runs Crowbar / Hydra]

&#x20;       A2\[📋 passwords.txt from rockyou.txt]

&#x20;       A3\[🔨 RDP Brute Force - Port 3389]

&#x20;       A1 --> A2 --> A3

&#x20;   end



&#x20;   subgraph TARGET\["💻 TARGET-PC - 192.168.10.100"]

&#x20;       B1\[🖥️ Windows 10 Pro]

&#x20;       B2\[👁️ Sysmon Monitoring]

&#x20;       B3\[📝 EventCode 4625 - Failed Logon]

&#x20;       B4\[✅ EventCode 4624 - Successful Logon]

&#x20;       B1 --> B2

&#x20;       B2 --> B3

&#x20;       B2 --> B4

&#x20;   end



&#x20;   subgraph FORWARDER\["📡 SPLUNK UNIVERSAL FORWARDER"]

&#x20;       C1\[Security Event Logs]

&#x20;       C2\[Sysmon Operational Logs]

&#x20;       C3\[System and Application Logs]

&#x20;       C1 --> C3

&#x20;       C2 --> C3

&#x20;   end



&#x20;   subgraph SIEM\["📊 SPLUNK SIEM - 192.168.10.10"]

&#x20;       D1\[📥 Receiving Port 9997]

&#x20;       D2\[🗂️ Index: endpoint]

&#x20;       D3\[🔍 SPL Query Analysis]

&#x20;       D4\[🚨 Brute Force Pattern Detected]

&#x20;       D1 --> D2 --> D3 --> D4

&#x20;   end



&#x20;   subgraph DC\["🏢 ADDC01 - 192.168.10.7"]

&#x20;       E1\[Windows Server 2022]

&#x20;       E2\[Active Directory DS]

&#x20;       E3\[Domain: demodomain.local]

&#x20;       E4\[Users: jsmith / tsmith - IT OU]

&#x20;       E1 --> E2 --> E3 --> E4

&#x20;   end



&#x20;   subgraph ART\["🧪 ATOMIC RED TEAM - TARGET-PC"]

&#x20;       F1\[T1136.001 - Create Local Account]

&#x20;       F2\[MITRE ATT\&CK Simulation]

&#x20;       F3\[3932 Events Captured in Splunk]

&#x20;       F1 --> F2 --> F3

&#x20;   end



&#x20;   A3 -->|RDP Brute Force| B1

&#x20;   B3 -->|Port 9997| D1

&#x20;   B4 -->|Port 9997| D1

&#x20;   C3 -->|Ships logs| D1

&#x20;   E4 -->|Domain Auth| B1

&#x20;   F3 -->|Detected| D2



&#x20;   style KALI fill:#1a1a2e,stroke:#e94560,stroke-width:2px,color:#fff

&#x20;   style TARGET fill:#16213e,stroke:#2d6a4f,stroke-width:2px,color:#fff

&#x20;   style FORWARDER fill:#0f3460,stroke:#533483,stroke-width:2px,color:#fff

&#x20;   style SIEM fill:#533483,stroke:#e94560,stroke-width:2px,color:#fff

&#x20;   style DC fill:#1a1a2e,stroke:#0078D4,stroke-width:2px,color:#fff

&#x20;   style ART fill:#16213e,stroke:#00b4d8,stroke-width:2px,color:#fff

&#x20;   style A1 fill:#e94560,stroke:#fff,color:#fff

&#x20;   style A2 fill:#e94560,stroke:#fff,color:#fff

&#x20;   style A3 fill:#e94560,stroke:#fff,color:#fff

&#x20;   style B1 fill:#2d6a4f,stroke:#fff,color:#fff

&#x20;   style B2 fill:#2d6a4f,stroke:#fff,color:#fff

&#x20;   style B3 fill:#e94560,stroke:#fff,color:#fff

&#x20;   style B4 fill:#2d6a4f,stroke:#fff,color:#fff

&#x20;   style C1 fill:#0f3460,stroke:#fff,color:#fff

&#x20;   style C2 fill:#0f3460,stroke:#fff,color:#fff

&#x20;   style C3 fill:#0f3460,stroke:#fff,color:#fff

&#x20;   style D1 fill:#533483,stroke:#fff,color:#fff

&#x20;   style D2 fill:#533483,stroke:#fff,color:#fff

&#x20;   style D3 fill:#533483,stroke:#fff,color:#fff

&#x20;   style D4 fill:#e94560,stroke:#fff,color:#fff

&#x20;   style E1 fill:#0078D4,stroke:#fff,color:#fff

&#x20;   style E2 fill:#0078D4,stroke:#fff,color:#fff

&#x20;   style E3 fill:#0078D4,stroke:#fff,color:#fff

&#x20;   style E4 fill:#0078D4,stroke:#fff,color:#fff

&#x20;   style F1 fill:#00b4d8,stroke:#fff,color:#fff

&#x20;   style F2 fill:#00b4d8,stroke:#fff,color:#fff

&#x20;   style F3 fill:#00b4d8,stroke:#fff,color:#fff

```



\## 🛠️ Technologies Used



\- \*\*VMware Workstation Pro\*\* — Virtualization platform for all VMs

\- \*\*Windows Server 2022\*\* — Active Directory Domain Services and Domain Controller

\- \*\*Windows 10 Pro\*\* — Target domain-joined workstation

\- \*\*Kali Linux\*\* — Attack machine running Crowbar and Hydra

\- \*\*Ubuntu Server 22.04\*\* — Hosts Splunk Enterprise SIEM

\- \*\*Splunk Enterprise 10.2.3\*\* — Log ingestion, indexing, and attack analysis

\- \*\*Splunk Universal Forwarder\*\* — Ships Windows event logs to Splunk

\- \*\*Sysmon by Sysinternals\*\* — Deep endpoint telemetry using olafhartong config

\- \*\*Crowbar\*\* — RDP brute force attack tool

\- \*\*Hydra\*\* — Network login cracker

\- \*\*Atomic Red Team\*\* — MITRE ATT\&CK adversary technique simulation

\- \*\*PowerShell\*\* — Active Directory automation and scripting



\---



\## 🔍 Detection Logic



\### Key Event IDs Monitored



| Event ID | Description | Significance |

|---|---|---|

| 4625 | Failed logon attempt | Flood of these = brute force indicator |

| 4624 | Successful logon | After flood = account compromise |

| 4720 | New user account created | Atomic Red Team T1136.001 |

| 4672 | Special privileges assigned | Privilege escalation indicator |



\### Splunk SPL Queries Used

index=endpoint EventCode=4625

index=endpoint EventCode=4624

index=endpoint EventCode=4625 | stats count by Account\_Name

index=endpoint host="TARGET-PC" | table \_time, Account\_Name, Source\_Network\_Address, EventCode

index=endpoint NewLocalAccount



\---



\## ⚔️ Attack Commands



\### Brute Force from Kali Linux

```bash

crowbar -b rdp -u tsmith -C passwords.txt -s 192.168.10.100/32

hydra -t 1 -V -f -l tsmith -P passwords.txt rdp://192.168.10.100

```



\### Atomic Red Team on TARGET-PC

```powershell

Invoke-AtomicTest T1136.001

```



\---



\## 📸 Screenshots



\### VMware — Network Configuration

\*Screenshot coming soon\*



\### Windows Server 2022 — Server Manager

\*Screenshot coming soon\*



\### Windows 10 — Static IP and Ping Success

\*Screenshot coming soon\*



\### Kali Linux — Desktop and Static IP

\*Screenshot coming soon\*



\### Ubuntu Server — Splunk Server IP Confirmed

\*Screenshot coming soon\*



\### Splunk — Dashboard

\*Screenshot coming soon\*



\### Splunk — Receiving Port 9997 Configured

\*Screenshot coming soon\*



\### Splunk — Endpoint Index Created

\*Screenshot coming soon\*



\### Sysmon — Installed on Windows Server

\*Screenshot coming soon\*



\### Sysmon — Installed on TARGET-PC

\*Screenshot coming soon\*



\### Splunk — Events Flowing from Both Machines

\*Screenshot coming soon\*



\### Active Directory — AD DS Role Selected

\*Screenshot coming soon\*



\### Active Directory — Server Promoted to Domain Controller

\*Screenshot coming soon\*



\### Active Directory — Domain Login Screen

\*Screenshot coming soon\*



\### Active Directory — Users jsmith and tsmith in IT OU

\*Screenshot coming soon\*



\### Domain Join — TARGET-PC Joined to demodomain.local

\*Screenshot coming soon\*



\### Domain Join — Logged in as Domain User jsmith

\*Screenshot coming soon\*



\### Remote Desktop — Enabled with Domain Users Allowed

\*Screenshot coming soon\*



\### Brute Force — Hydra Attack Against tsmith

\*Screenshot coming soon\*



\### Splunk — Failed Logins Detected EventCode 4625

\*Screenshot coming soon\*



\### Splunk — tsmith Account Events

\*Screenshot coming soon\*



\### Splunk — Successful Login Detected EventCode 4624

\*Screenshot coming soon\*



\### Splunk — Event Details with Source IP

\*Screenshot coming soon\*



\### Atomic Red Team — Installed on TARGET-PC

\*Screenshot coming soon\*



\### Atomic Red Team — T1136.001 Test Running

\*Screenshot coming soon\*



\### Splunk — 3932 Events Captured from TARGET-PC

\*Screenshot coming soon\*



\### Splunk — New Local Account Event T1136.001 Confirmed

\*Screenshot coming soon\*



\---



\## 🚀 How to Reproduce This Lab



\### Prerequisites

\- VMware Workstation Pro

\- 16GB RAM minimum on host machine

\- Windows Server 2022 ISO

\- Windows 10 Pro ISO

\- Kali Linux ISO

\- Ubuntu Server 22.04 ISO



\### Step 1 — Configure VMware Network

Set VMnet8 to NAT with subnet 192.168.10.0/24 in Virtual Network Editor.



\### Step 2 — Create All Four VMs

Create ADDC01, TARGET-PC, KALI-ATTACKER, and SPLUNK-SERVER with static IPs per the architecture table above.



\### Step 3 — Install Splunk on Ubuntu Server

```bash

wget -O splunk.deb "YOUR\_SPLUNK\_DOWNLOAD\_URL"

sudo dpkg -i splunk.deb

sudo /opt/splunk/bin/splunk start --accept-license

```



\### Step 4 — Install Sysmon on Windows Machines

```powershell

.\\Sysmon64.exe -accepteula -i sysmonconfig.xml

```



\### Step 5 — Install Splunk Universal Forwarder

Install on both Windows machines pointing to 192.168.10.10:9997. Use inputs.conf from the configs folder in this repo.



\### Step 6 — Promote Domain Controller

Server Manager > Add Roles > AD DS > Promote to Domain Controller

Forest: demodomain.local



\### Step 7 — Join Windows 10 to Domain

System Properties > Change > Domain > demodomain.local

Credentials: DEMODOMAIN\\Administrator



\### Step 8 — Run the Attack from Kali

```bash

crowbar -b rdp -u tsmith -C passwords.txt -s 192.168.10.100/32

hydra -t 1 -V -f -l tsmith -P passwords.txt rdp://192.168.10.100

```



\### Step 9 — Analyze in Splunk

index=endpoint EventCode=4625

index=endpoint EventCode=4624

index=endpoint EventCode=4625 | stats count by Account\_Name



\---



\## 📁 Repository Structure

Active\_Directory\_Lab/

├── configs/

│   └── inputs.conf

├── screenshots/

│   └── 27 step-by-step screenshots

├── scripts/

│   └── create-users.ps1

└── README.md



\## 🎯 Skills Demonstrated



\- Virtualization and VM lifecycle management with VMware

\- Active Directory domain setup, OU structure, and user management

\- Static IP addressing, DNS configuration, and network segmentation

\- SIEM deployment and configuration with Splunk Enterprise

\- Endpoint telemetry collection using Sysmon and Splunk Universal Forwarder

\- Offensive security — RDP brute force simulation using Crowbar and Hydra

\- Defensive security — attack detection using Windows Event IDs 4624 and 4625

\- MITRE ATT\&CK framework technique simulation with Atomic Red Team

\- PowerShell scripting for Active Directory user automation

\- Linux server administration and Netplan network configuration



\---



\## 👤 Author



\*\*Ojas Yajnik\*\*

Cybersecurity enthusiast building hands-on security labs



\[!\[LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://www.linkedin.com/in/ojasyajnik)

\[!\[GitHub](https://img.shields.io/badge/GitHub-Follow-black)](https://github.com/ojasy)



\---



\## 📜 License



MIT License — feel free to use this for your own learning.

