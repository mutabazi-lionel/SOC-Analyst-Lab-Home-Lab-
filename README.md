# SOC-Analyst-Lab-Home-Lab
This project simulates a real-world Security Operations Center (SOC) environment for blue teaming, detection engineering, and adversary emulation. The lab is designed and operated using personal infrastructure and open-source tools, focused on improving practical cybersecurity skills.

🧱 **Lab Architecture Overview**

                       [ Internet ]
                            |
                   [ Ubuntu Attacker ]
                         (eth0)
                            |
             ┌──────────── NAT/Firewall ─────────────┐
             |                                       |
        [ Internal Virtual Network / 10.0.0.0/24 ]   |
             |                                       |
   ┌────────────────────────────────────────────────────┐
   |  [ Win Server (AD) ] ←→ [ Win Client ]             |
   |        ↑                        ↑                  |
   |     (Wazuh Agent)          (Wazuh Agent)           |
   |         ↓                        ↓                 |
   |        [ Wazuh/Splunk Server — Log Collector ]     |
   └────────────────────────────────────────────────────┘

### 🧩 Lab Components

### 1. Ubuntu Attacker (External)

==> Role: Simulated attacker from the internet

### 2. Windows Server (AD)

* Role: Domain Controller
* Configuration:

  * Active Directory, DNS
  * User/group policy management
  * Wazuh agent + Sysmon installed

### 3. Windows Client (Workstation)

* Role: Domain-joined client machine
* Tools:
  * Sysmon, Winlogbeat
  * Wazuh agent
  * Vulnerable software for testing

### 4. Wazuh & Splunk Server

* Role: Log collection, analysis, and alerting
* Stack:
  * Wazuh manager (SIEM)
  * Filebeat (optional Splunk forwarding)
  * Splunk (free license, 500MB/day)
  * Wazuh and Windows add-ons for Splunk

## 🎯 Attack Scenarios Simulated

### 🔐 Initial Access

* Simulated phishing via Python HTTP server
* Delivered malicious documents (macro-based payloads)
* Reverse shell access from client to Ubuntu attacker

### 📈 Privilege Escalation

* Used WinPEAS and PowerUp for enumeration
* Escalated via service misconfigurations and credential reuse

### 🔁 Lateral Movement

* Tools: `psexec.py`, `wmiexec.py`, `evil-winrm`
* Pivoted from client to domain controller
* Executed commands and moved data across systems

### 🎩 Active Directory Attacks

* Kerberoasting with `GetUserSPNs.py`
* LDAP enumeration via `ldapdomaindump`
* Used BloodHound to map AD relationships

## 🛡️ Detection & Logging

### Tools Used

* Sysmon (with SwiftOnSecurity config)
* Wazuh (alerting + file integrity monitoring)
* Winlogbeat/Filebeat (log shippers)
* Splunk for dashboarding and correlation

### Key Events Monitored

| Technique            | Event ID / Detection       |
| -------------------- | -------------------------- |
| RDP Brute Force      | 4625, 4624                 |
| PowerShell Execution | 4104 (Sysmon)              |
| Credential Dumping   | 10 (Sysmon), 4688          |
| Kerberoasting        | 4769                       |
| Lateral Movement     | 7045, 5140, service create |

### 📊 Dashboards & Alerts

* Created custom Splunk dashboards:

  * Top failed logins
  * Powershell usage
  * New service creations
* Wazuh rules tuned to reduce false positives
* Email alerts for high-severity threats

## 📁 Sample Files & Configurations

* `wazuh-agent.conf`: Windows agent configuration
* `sysmon-config.xml`: Sysmon rules
* `splunk-alert-rules.conf`: Detection rule samples
* Screenshots and logs from simulated attacks

## 🔄 Future Improvements

* Integrate TheHive for case management
* Add Suricata for network-based detection
* Use Velociraptor or Osquery for deeper EDR
* Automate attack simulation with Atomic Red Team

---

## 🧠 Learning Outcomes

* Deploying and managing a working SOC environment
* Understanding and applying MITRE ATT\&CK techniques
* Monitoring endpoint and domain controller activity
* Writing detection rules and investigating security events
* Simulating realistic attacks and testing defense tools

## 📎 License

This project is for educational and non-commercial use only.

---

## 🙌 Acknowledgements

* MITRE ATT\&CK Framework
* Wazuh Community
* SwiftOnSecurity Sysmon config
* Splunk Security Essentials
* BloodHound AD Tools

