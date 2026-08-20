# Module 5: Incident & Change Management

## IR Lifecycle, ServiceNow ITSM & CAB Change Process

Incident Management ensures security events are tracked, escalated and resolved in a structured way. Change Management ensures that modifications to IT infrastructure are planned, approved and documented to prevent outages and security gaps.

---

# Lab 5.1 — Incident Response Lifecycle

## Objective

**Execute the full 6-phase IR lifecycle on a simulated incident.**

---

## Step 1: Preparation Phase

Maintain operational readiness before an incident occurs.

* Maintain an updated **asset inventory**.
* Maintain updated contact lists for:

  * Legal
  * HR
  * PR
  * IT leaders
* Ensure IR tools are ready:

  * Forensic workstation
  * Write blockers
  * Bootable media
* Conduct **quarterly tabletop exercises (TTX)** covering:

  * Ransomware
  * Phishing
  * Insider threat
* Review and update IR playbooks **semi-annually**.
* Confirm backup integrity and ensure restoration procedures have been tested.

> 💡 **Preparation Principle:** Incident response effectiveness depends heavily on the readiness of people, processes, tools, and recovery resources before an incident occurs.

---

## Step 2: Identification Phase

1. A security alert fires in the SIEM.

2. Assign the incident to the **Incident Commander (IC)**.

3. The IC declares the incident severity:

   | Severity | Classification |
   | -------- | -------------- |
   | **P1**   | Critical       |
   | **P2**   | High           |
   | **P3**   | Medium         |
   | **P4**   | Low            |

4. Open a formal incident ticket in **ServiceNow / JIRA** containing:

   * Timestamp
   * Description
   * Affected systems

5. Establish a **War Room** communication channel:

```text
Teams/Slack #incident-
```

6. Notify stakeholders according to incident severity:

   * **P1** → All hands
   * **P2** → IR team
   * **P3/P4** → L2 analyst

---

## Step 3: Containment Phase

### Short-Term Containment

Isolate affected host(s) using appropriate controls:

* NAC quarantine
* Disable switch port
* Block traffic at the firewall

Disable the affected Active Directory account where required:

```powershell
Disable-ADAccount -Identity <account>
```

### Block Indicators of Compromise

Block known IOCs, including:

* C2 IP addresses
* Malicious domains

Use:

* Firewall
* Proxy
* DNS sinkhole

### Long-Term Containment

Where applicable:

* Patch the known vulnerability.
* Rotate compromised credentials.
* Segment the affected VLAN.

> ⚠️ **Evidence Preservation:** Preserve evidence before performing any containment action that might destroy or modify it.

---

## Step 4: Eradication & Recovery Phase

### Remove Malware

Re-image compromised hosts using a **known-good image**.

### Validate

Before reconnecting a re-imaged host to the network:

* Scan with **AV**.
* Scan with **EDR**.

### Restore from Backup

Restore affected systems from backup.

> 🔐 **Integrity Requirement:** Verify the hash integrity of the backup before performing the restore.

### Monitor

Increase alert sensitivity for **72 hours post-recovery**.

### Document

Document all actions taken, including timestamps, for the post-incident review.

---

## Step 5: Lessons Learned (Post-Incident Review)

### Schedule the PIR

Schedule a **Post-Incident Review (PIR)** within **5 business days** of incident closure.

### Discussion

Address:

* What happened?
* What was the root cause?
* What worked?
* What didn't?

### Document Findings

Document:

* Gaps in detection
* Response delays
* Missing playbooks

### Assign Action Items

Assign:

* An owner for each identified gap.
* A deadline for each corrective action.

### Update Security Controls

Update the following based on the findings:

* Playbooks
* Detection rules
* Hardening standards

---

## ⭐ Best Practice Tips

* **Document everything in real-time during an incident** — memory fades fast under pressure.
* **Separate the investigation lead from the Incident Commander** — one investigates, one coordinates.
* **Never communicate breach details via email before legal review** — privilege considerations.
* **Time-box your decisions** — 80% confidence in 20 minutes beats 100% confidence in 2 hours during an active attack.
* **Regulatory notification deadlines** (GDPR 72hr, SEC 4 business days) start from discovery, not closure.
