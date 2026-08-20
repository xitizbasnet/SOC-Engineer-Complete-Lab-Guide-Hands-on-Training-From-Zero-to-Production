# Lab 7.2 — EDR Alert Investigation

## Objective

**Investigate a Falcon detection from alert to conclusion.**

---

## Step 1: Review Detection Alert

1. Navigate to:

   **Endpoint Security** → **Detections**

2. Select the relevant detection.

3. Review the following information:

   * **Severity**
   * **MITRE Tactic/Technique**
   * **Affected host**
   * **User**
   * **Process**

### Review the Process Tree

Examine the parent-child process chain that led to the detection.

> 🔎 **Investigation Objective:** Determine how the suspicious process was initiated and identify the sequence of processes leading to the detection.

### Review the Timeline

Review the complete sequence of events:

* Events before the detection.
* The detection event.
* Events after the detection.

### Establish Context

Check:

* Is the host a known high-value asset?
* Was the user on shift?

---

## Step 2: Investigate in Falcon Insight — Threat Hunting

Navigate to:

**Investigate** → **Falcon Insight** → **Event Search**

### Search for PowerShell Execution

Use the following event query:

```text
event_simpleName=ProcessRollup2 FileName=powershell.exe
```

### Filter by Host and Time

Use:

```text
aid=<host_id> timestamp>last24h
```

> 📝 Replace `<host_id>` with the appropriate endpoint identifier.

### Pivot During Investigation

Pivot on:

* File hash
* Parent process
* Network connections originating from the same host

For example:

```text
event_simpleName=NetworkConnectIP4 LocalAddressIP4=10.1.1.100
```

> 🔎 **Pivoting Principle:** Use related process, file, host, and network telemetry to determine whether the detection represents isolated activity or part of a broader attack chain.

---

## Step 3: Analyse Process Graph

1. Open the detection.

2. Navigate to:

   **Process Graph**

3. Review the visual representation of the process chain.

### Identify Unusual Parent-Child Relationships

Look for suspicious relationships, such as:

```text
Microsoft Word → PowerShell
```

> ⚠️ **Example:** Word spawning PowerShell may indicate macro execution.

### Review Command-Line Arguments

Check for:

* Encoded commands
* Download cradles

### Review File Events

Determine:

* What files were created?
* What files were modified?
* Which files were associated with the suspicious process?

### Review Network Activity

Determine:

* Did the process make outbound connections?
* Which IP addresses were contacted?

---

## Step 4: Verdict & Action

### ✅ True Positive

If the detection is confirmed as malicious:

1. Escalate the incident.

2. Set detection status to:

   **In Progress**

3. Assign the investigation to **L2**.

### 🛑 Contain the Host

Use:

**Actions** → **Contain**

This performs network isolation while allowing the endpoint to continue communicating with the **Falcon cloud**.

> ⚠️ **Important:** Network containment does not necessarily prevent lateral movement through domain trust. Also disable the associated Active Directory account when appropriate.

### 📥 Collect Suspicious Files

Use:

**Actions** → **RTR** → **Download**

Download the suspicious file for further analysis.

### ❌ False Positive

If the detection is determined to be benign:

1. Set the status to:

   **False Positive**

2. Add an exclusion if appropriate and authorised.

### 📝 Document the Investigation

Record investigation notes in the **detection comment field**.

Include:

* Evidence reviewed
* Investigation findings
* Verdict
* Actions taken
* Escalation details
* Any exclusions or remediation performed

---

## ⭐ Best Practice Tips

* **Always read the full process command line** — the truncated console view may omit important arguments.
* **Check hash reputation in VirusTotal AND Falcon Intel** before concluding that a detection is a false positive.
* **Network containment does NOT prevent lateral movement via domain trust** — also disable the AD account where appropriate.
* **Export the process graph and timeline as evidence** before performing remediation.
* **Create Custom IOA rules from confirmed TP detections** to detect variants without relying solely on ML signatures.
