# Lab 1.4 — Threat Hunting in Splunk

## Objective

**Proactively hunt for threats using hypothesis-driven SPL queries.**

---

## Step 1: Hunt for Pass-the-Hash (PTH) Attacks

### 🎯 Hypothesis

> An attacker is using NTLM hashes to authenticate laterally.

Run the following SPL query:

```spl
index=security_logs EventCode=4624 Logon_Type=3 Authentication_Package=NTLM
| stats count by user, src_ip, ComputerName
| sort -count
```

### 🔎 Investigation Focus

* Look for **service accounts authenticating from unusual workstations**.
* Review authentication patterns for unexpected source IP addresses and computer names.
* Investigate accounts showing unusual lateral authentication activity.

---

## Step 2: Hunt for PowerShell Abuse

Search for PowerShell process execution using the following SPL query:

```spl
index=security_logs sourcetype=WinEventLog:Security EventCode=4688
process=*powershell*
| table _time, user, CommandLine, ComputerName
```

### 🚩 Indicators to Flag

Look for the following PowerShell command-line indicators:

* `-EncodedCommand`
* `-WindowStyle Hidden`
* `IEX`
* `DownloadString`
* `Bypass`

Search PowerShell Script Block Logging events for potentially suspicious activity:

```spl
index=security_logs EventCode=4104
| search ScriptBlockText=*DownloadString* OR ScriptBlockText=*Invoke-Expression*
```

---

## Step 3: Hunt for Persistence Mechanisms

### Scheduled Tasks

Search for newly created scheduled tasks:

```spl
index=security_logs EventCode=4698
| table _time, user, TaskName, TaskContent
```

### Registry Run Keys

Search for registry Run Key activity:

```spl
index=security_logs EventCode=13 TargetObject=*Run*
| table _time, user, TargetObject, Details
```

### New Services

Search for newly created Windows services:

```spl
index=security_logs EventCode=7045
| table _time, user, ServiceName, ImagePath, StartType
```

> 🔎 **Investigation Tip:** Persistence mechanisms should be reviewed in context. Validate whether the associated account, executable, task, registry modification, or service is expected within the environment.

---

## Step 4: Document Hunt Findings

For every threat hunt, document the following:

* **Hypothesis**
* **Query used**
* **Timeframe**
* **Findings**
* **False positive rate**

### Confirmed Threat

If a confirmed threat is identified:

**Escalate as Incident** → **Follow IR playbook**

### No Finding

If no threat is identified:

**Update baseline / normal behaviour documentation**

### Successful Hunt

Convert successful threat hunts into **automated correlation rules** where appropriate.

---

## ⭐ Best Practice Tips

* **Always start a hunt with a clear hypothesis** tied to a **MITRE ATT&CK** technique.
* **Use the kill chain model** to identify the attack stage and pivot from there.
* **Establish baselines first** — you can't spot anomalies without knowing normal behaviour.
* **Share hunt results with the team** — successful hunts become tomorrow's detection rules.
* **Use Splunk's ML Toolkit** for anomaly detection on time-series data.
