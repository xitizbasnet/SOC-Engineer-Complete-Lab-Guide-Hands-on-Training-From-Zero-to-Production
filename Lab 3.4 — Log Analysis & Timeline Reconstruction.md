# Lab 3.4 — Log Analysis & Timeline Reconstruction

## Objective

**Correlate multi-source logs to reconstruct an attack timeline.**

---

## Step 1: Collect All Relevant Logs

Collect and preserve logs from all relevant systems and security controls.

### Windows Event Logs

Collect:

* `Security.evtx`
* `System.evtx`
* `Application.evtx`
* `PowerShell.evtx`

Export the Security event log:

```powershell
wevtutil epl Security C:\evidence\Security.evtx
```

### Linux Logs

Collect:

```text
/var/log/auth.log
/var/log/syslog
/var/log/audit/audit.log
```

### Network Logs

Collect:

* Firewall logs
* Proxy logs
* DNS logs
* VPN logs

### Application Logs

Collect:

* Web server access logs (**Apache/Nginx**)
* Database audit logs

> 🔐 **Evidence Handling:** Preserve original log files unchanged and perform analysis on working copies.

---

## Step 2: Parse Windows Event Logs

### Key Event IDs

| Event ID | Description             |
| -------: | ----------------------- |
|   `4624` | Login                   |
|   `4625` | Failed Login            |
|   `4648` | Explicit Credential Use |
|   `4688` | Process Created         |
|   `4698` | Scheduled Task          |
|   `4720` | Account Created         |
|   `7045` | Service Installed       |
|   `1102` | Audit Log Cleared       |

### Search for Failed Logins

Use PowerShell to identify failed login events:

```powershell
Get-WinEvent -Path Security.evtx | Where-Object {$_.Id -eq 4625} | Export-Csv logins.csv
```

### Analyse Windows Event Logs with Chainsaw

Use **Chainsaw**, a Rust-based tool for fast Windows event log analysis:

```bash
./chainsaw hunt Security.evtx --rules rules/ --mapping mappings/sigma-event-logs-all.yml
```

> 💡 **Tip:** Sigma-based analysis can help identify attacker TTPs across Windows event logs.

---

## Step 3: Correlate Logs into Timeline with Plaso

Install Plaso:

```bash
pip install plaso --break-system-packages
```

Parse the available evidence and create a Plaso timeline:

```bash
log2timeline.py /evidence/timeline.plaso /evidence/
```

Export the timeline to CSV:

```bash
psort.py -o dynamic -w timeline.csv /evidence/timeline.plaso
```

### Analyse the Timeline with Timesketch

Open the resulting timeline in **Timesketch**, a web UI for collaborative timeline analysis.

For focused analysis:

* Filter around **T-1 hour** of the confirmed incident.
* Filter around **T+1 hour** of the confirmed incident.

> 🔎 **Investigation Focus:** Correlate events from multiple sources to identify the sequence of attacker actions before, during, and after the confirmed incident.

---

## Step 4: Write Forensic Report

The forensic report should contain the following sections.

### Executive Summary

Document:

* What happened
* When it happened
* Impact
* Attacker objective

### Technical Findings

Document:

* Evidence collected
* Analysis method
* Each artefact and its significance

### Timeline

Provide a **chronological sequence of attacker actions**.

### IOCs

Document all identified indicators of compromise, including:

* IP addresses
* Domains
* Hashes
* File paths
* Registry keys

### Recommendations

Document:

* Detection gaps
* Remediation steps
* Control improvements

> ⚖️ **Forensic Reporting:** Use passive voice and factual language — forensic reports may be used in legal proceedings.

---

## ⭐ Best Practice Tips

* **Synchronise all timestamps to UTC** before correlating multi-source logs.
* **Look for Event ID `1102` (Audit Log Cleared)** — a major red flag indicating potential anti-forensics activity.
* **Use Sigma rules** to automatically search event logs for attacker TTPs.
* **Cross-reference authentication events with access control lists** — determine whether the access was authorised.
* **Preserve original log files unchanged** — work on copies only.
