# SOC Engineer — Skills Summary

This section provides a consolidated overview of the technical skills, primary tools, universal SOC practices, and certification roadmap covered throughout the SOC training program.

---

## Skills & Primary Tools

| Module                | Key Skills                                                          | Primary Tools                               |
| --------------------- | ------------------------------------------------------------------- | ------------------------------------------- |
| **SIEM**              | Log ingestion, correlation rules, threat hunting, dashboards        | Splunk Enterprise<br>Splunk ES              |
| **Malware Analysis**  | Static PE analysis, dynamic sandbox, memory forensics               | PEStudio<br>ANY.RUN<br>Volatility 3         |
| **Digital Forensics** | Evidence acquisition, disk forensics, network analysis, timeline    | Autopsy<br>Sleuth Kit<br>Wireshark<br>Plaso |
| **SOC Operations**    | Alert triage, threat intel, playbook execution                      | SIEM/SOAR<br>MISP<br>ATT&CK Navigator       |
| **Incident & Change** | IR lifecycle, ITSM ticketing, CAB change process                    | ServiceNow<br>JIRA                          |
| **PAM**               | Credential vaulting, session recording, JIT access, least privilege | CyberArk PAS<br>EPM                         |
| **CrowdStrike**       | EDR investigation, containment, real-time response                  | Falcon Console<br>Falcon Insight            |
| **Zscaler**           | URL filtering, SSL inspection, ZTNA, DLP                            | ZIA Admin<br>ZPA Admin                      |
| **Fortinet**          | NGFW policy, log analysis, EDR investigation                        | FortiGate<br>FortiAnalyzer<br>FortiEDR      |
| **Palo Alto**         | NGFW policy, XDR investigation, WildFire, IPS                       | Panorama<br>Cortex XDR                      |

---

# 🛡️ Universal SOC Best Practices

The following practices apply across all SOC functions, technologies, and operational workflows.

### 1. Document Everything

> **If it isn't written down, it didn't happen** — in a SOC or courtroom.

Maintain clear records of:

* Investigations
* Decisions
* Evidence
* Response actions
* Escalations
* Changes
* Lessons learned

### 2. Adopt an Assume-Breach Mentality

Monitor **east-west (internal)** traffic just as closely as **north-south** traffic.

> 🔎 Attackers who bypass perimeter controls can move laterally within the environment. Internal visibility is therefore essential.

### 3. Operationalise Threat Intelligence

> **Threat intel without operationalisation is just information.**

Automate IOC ingestion into:

* SIEM
* Firewall
* EDR

Use threat intelligence to drive detection, investigation, and response rather than treating feeds as passive information sources.

### 4. Manage SOC Burnout

> **Burnout is the #1 SOC problem.**

Reduce analyst fatigue by:

* Rotating shifts
* Cross-training analysts
* Automating repetitive tasks with SOAR
* Standardising playbooks
* Sharing knowledge across the SOC

### 5. Measure What Matters

Track operational metrics weekly, including:

| Metric                  | Purpose                             |
| ----------------------- | ----------------------------------- |
| **MTTD**                | Mean Time to Detect                 |
| **MTTR**                | Mean Time to Respond                |
| **False Positive Rate** | Measures detection quality          |
| **SLA Compliance**      | Measures operational responsiveness |

### 6. Treat Security as a Team Sport

Share relevant IOCs and threat information with:

* ISACs
* Industry peers
* Trusted security communities
* The broader security community

### 7. Prioritise Depth Over Breadth

> **The best security tool is the one properly configured and maintained.**

A smaller, well-managed security stack is more valuable than a large collection of poorly configured tools.

### 8. Practice Incident Response

> **Practice incident response before you need it.**

Conduct:

* Tabletop exercises
* Live-fire exercises
* Playbook testing
* Recovery exercises

Regular practice builds operational **muscle memory** before a real incident occurs.

---

# 🎓 Certification Roadmap for SOC Engineers

The following roadmap provides a progression from foundational security knowledge through advanced SOC engineering, digital forensics, malware analysis, and security leadership.

| Level        | Certification                             | Focus Area                               |
| ------------ | ----------------------------------------- | ---------------------------------------- |
| **Entry**    | **CompTIA Security+**                     | Security fundamentals                    |
| **L1 SOC**   | **CompTIA CySA+**                         | Threat analysis, SIEM, log review        |
| **L2 SOC**   | **EC-Council CEH / eJPT**                 | Offensive knowledge, penetration basics  |
| **L2 SOC**   | **Splunk Core Certified User/Power User** | SIEM — Splunk SPL mastery                |
| **L2 SOC**   | **CrowdStrike CCFA / CCFH**               | Falcon administration and threat hunting |
| **L3 SOC**   | **GIAC GCFE / GCFA**                      | Digital forensics and incident response  |
| **L3 SOC**   | **GIAC GREM**                             | Malware reverse engineering              |
| **Advanced** | **OSCP (Offensive Security)**             | Adversary emulation for SOC context      |
| **Advanced** | **CISSP / CISM**                          | Security management and strategy         |

---

## 🧭 Suggested SOC Engineer Progression

```text
Security Fundamentals
        │
        ▼
     L1 SOC
        │
        ├── SIEM & Log Analysis
        ├── Alert Triage
        └── Threat Intelligence
        │
        ▼
     L2 SOC
        │
        ├── Threat Hunting
        ├── EDR/XDR
        ├── Incident Response
        └── Offensive Security Knowledge
        │
        ▼
     L3 SOC
        │
        ├── Digital Forensics
        ├── Malware Analysis
        ├── Advanced Threat Hunting
        └── Detection Engineering
        │
        ▼
   Advanced / Senior
        │
        ├── Adversary Emulation
        ├── Security Architecture
        ├── Automation & SOAR
        └── Security Management & Strategy
```

> 🎯 **Core Outcome:** A well-rounded SOC Engineer should be able to **detect → investigate → contain → eradicate → recover → document → improve**, while operating effectively across SIEM, EDR/XDR, network security, PAM, threat intelligence, forensics, and ITSM platforms.
