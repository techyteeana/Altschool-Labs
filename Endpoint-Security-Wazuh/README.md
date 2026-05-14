# 🛡️ Endpoint Security Monitoring: Wazuh SIEM Implementation
**Project by: @techyteeana** | **AltSchool Africa Cybersecurity**

## 📌 Project Overview
This project involved deploying and configuring a **Security Information and Event Management (SIEM)** solution using Wazuh. I successfully integrated a Windows endpoint into a Linux-based Wazuh Manager to perform real-time **File Integrity Monitoring (FIM)** and security event analysis.

---

## 🛠️ Tech Stack & Skills
* **SIEM Platform:** Wazuh 4.x (Manager & Indexer)
* **Environment:** Ubuntu Linux (Server), Windows 10 (Agent)
* **Virtualization:** Oracle VirtualBox
* **Security Protocols:** File Integrity Monitoring (FIM), Syscheck
* **Tools used:** PowerShell, XML Configuration, CLI, Log Analysis

---

## 🔍 The "Manager vs. Agent" Troubleshooting Lesson
A key part of this project was a deep dive into the architecture of a SIEM. I initially struggled to get the monitoring frequency to update.

### The Problem
I wanted to reduce the check interval to **60 seconds** for near real-time alerts. I spent significant time editing the `ossec.conf` file on the **Linux Wazuh Server**, but the Windows host wouldn't respond to the changes.

### The Realization
I realized that while the Linux server *receives* the alerts, it doesn't automatically control the local security policy of a remote Windows Agent. The Windows agent follows the rules stored in its own local directory.

### The Fix
I pivoted from the Server to the **Windows Host** and performed the following:
1.  Launched **PowerShell as Administrator**.
2.  Modified the local agent configuration at `C:\Program Files (x86)\ossec-agent\ossec.conf`.
3.  Updated the `<syscheck>` frequency and directory paths.
4.  Restarted the service using `net stop wazuh` and `net start wazuh`.
**Result:** The agent immediately synced, and alerts began appearing on the dashboard in under a minute.

---

## 🧪 Verification & Testing (Proof of Concept)
To validate the monitoring setup, I performed a manual file modification to trigger a high-severity alert.

1.  **Action:** Edited `important_documentII.txt` within the monitored directory.
2.  **Detection:** The Wazuh Manager detected the change via the agent's `syscheck` daemon.
3.  **Analysis:** Verified the alert on the **Wazuh Dashboard** (Rule 550), which provided the exact timestamp, file path, and user account involved.

![Wazuh Dashboard Alert](./wazuh-alert.png)
*Above: Screenshot of the real-time FIM alert triggered during testing.*

---

## 🛡️ Skills Demonstrated
* **SIEM Administration:** Managed server-agent communication and resolved configuration sync issues.
* **Endpoint Hardening:** Configured agents to monitor sensitive system directories for unauthorized changes.
* **Incident Investigation:** Analyzed raw security events to verify system integrity.
* **Technical Documentation:** Documented a full deployment from installation to verification.

---

## 🚀 Final Reflection
This lab taught me that in a real-world SOC environment, understanding the flow of data between the **Manager** and the **Agent** is critical. I learned that troubleshooting at the "Source" (the endpoint) is often just as important as monitoring the "Destination" (the dashboard).
