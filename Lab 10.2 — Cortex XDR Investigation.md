# Lab 10.2 — Cortex XDR Investigation

## Objective

**Investigate cross-layer alerts in Cortex XDR.**

---

## Step 1: Cortex XDR Console Navigation

### Access the Cortex XDR Console

Navigate to:

```text
https://<tenant>.xdr.us.paloaltonetworks.com
```

> 📝 Replace `<tenant>` with the tenant identifier for your Cortex XDR environment.

### Key Console Areas

| Area              | Purpose                                                       |
| ----------------- | ------------------------------------------------------------- |
| **Incidents**     | Correlated alerts                                             |
| **Alerts**        | Raw detections                                                |
| **Investigation** | Threat hunting                                                |
| **Assets**        | Endpoint inventory, network sensors, and cloud workloads      |
| **Response**      | Isolate, scan, and execute scripts                            |
| **Dashboard**     | Executive summary, MTTD/MTTR metrics, and top affected assets |

> 🔎 **Response Note:** Cortex XDR response capabilities provide functionality similar to CrowdStrike Real Time Response (RTR).

---

## Step 2: Investigate an XDR Incident

### Open a High-Severity Incident

Navigate to:

**Incidents** → **Open High Severity Incident** → **Click to view incident**

### Review the Causality Graph

Use the **Causality Graph** to review the automatically built visual representation of related alerts across:

* Endpoint
* Network
* Cloud

Expand individual nodes to investigate:

```text
Process
   ↓
Network
   ↓
File Operations
```

This provides a unified view of related activity.

### Review the Incident Timeline

Use the **Timeline** to examine the chronological sequence of all events associated with the incident.

### Review MITRE ATT&CK Mapping

Open the:

**MITRE ATT&CK View**

Review which techniques were triggered across the attack kill chain.

> 💡 **Investigation Tip:** Start with the incident-level context and causality graph before drilling down into individual raw events.

---

## Step 3: Threat Hunting in XQL

### Open XQL Investigation

Navigate to:

**Investigation** → **XQL Query**

Use Cortex XQL to write and execute threat-hunting queries.

### PowerShell Process Hunt

```xql
dataset=xdr_data
| filter event_type='PROCESS' and
  action_process_image_name='powershell.exe'
| fields actor_process_command_line,
  hostname, _time
```

### Network Connection Hunt

```xql
dataset=xdr_data
| filter action_network_connection_id != null and
  actor_process_image_name!='chrome.exe'
| fields hostname, action_remote_ip, _time
```

### Save Successful Queries

Save successful hunting queries as:

> ⭐ **Starred Queries**

This allows analysts to reuse effective queries during future investigations.

### Schedule XQL Hunts

Configure successful XQL hunt queries to:

* Run daily.
* Automatically create an incident when results are found.

> 🤖 **Automation Benefit:** Scheduled hunting helps continuously monitor for suspicious patterns without requiring analysts to manually execute every query.

---

## Step 4: Endpoint Response Actions

### Isolate an Endpoint

Select the affected endpoint:

**Right-click** → **Isolate Endpoint**

This applies network isolation to the endpoint.

### Execute an Investigation Script

Navigate to:

**Response** → **Run Script**

Execute an appropriate:

* PowerShell investigation script
* Bash investigation script

### Retrieve a Suspicious File

Use **Retrieve File** to pull a suspicious file from the endpoint for further sandbox analysis.

### Scan the Endpoint

Use **Scan Endpoint** to trigger an on-demand:

* XDR antivirus scan
* Behavioral scan

### Use the Live Terminal

Use **Live Terminal** to obtain browser-based terminal access to the endpoint.

> 🖥️ **Comparison:** Live Terminal provides functionality similar to RTR for interactive endpoint investigation.

### Block a File Hash

Use **Block Hash** to prevent execution of a specific file hash across the endpoint fleet.

> ⚠️ **Caution:** Fleet-wide hash blocking can affect legitimate software if the indicator is incorrectly classified. Validate the hash before applying broad blocking actions.

---

## ⭐ Best Practice Tips

* **Cortex XDR Causality Graph is its biggest differentiator** — use it before looking at raw logs.
* **XQL is powerful** — build a library of detection queries and share across the SOC team.
* **Use Action Centre to track all response actions taken** — important for audit and IR report.
* **Enable Behavioral Threat Protection (BTP)** — catches novel malware without signatures.
* **Correlate Cortex XDR with Panorama firewall logs** for full kill chain reconstruction.
