# Lab 7.3 — Real-Time Response & Containment

## Objective

**Use RTR to investigate and remediate a compromised endpoint.**

---

## Step 1: Initiate RTR Session

1. Navigate to:

   **Response** → **Real Time Response** → **Hosts**

2. Select the target host.

3. Select:

   **Start Session**

RTR opens a terminal-like interface directly to the endpoint without requiring a VPN.

> 🔐 **Audit Requirement:** RTR sessions are logged, and all commands are audited in Falcon.

### RTR Session Characteristics

* The session operates even if the host is network contained.
* Multiple analysts can view the session.
* Only one analyst can execute commands at a time.

> 📝 **Operational Note:** Coordinate command execution between analysts to prevent conflicting actions during an active investigation.

---

## Step 2: Common RTR Investigation Commands

Use RTR commands to examine processes, network connections, persistence mechanisms, suspicious files, and command history.

### Process Investigation

List running processes:

```text id="x6j7lc"
ps
```

### Network Investigation

View active network connections:

```text id="0r8xmb"
netstat
```

### Registry Persistence

Check the Windows Run key:

```text id="2ts7p0"
reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run
```

### Check Common Malware Drop Locations

```text id="z1hx5m"
ls C:\Users\Public\
```

### Review Command History

```text id="z9z2u3"
history
```

Review the command history associated with the suspect user session.

### Download a Suspicious File

```text id="qg0e1v"
get C:\Temp\suspicious.exe
```

> 🔎 **Investigation Objective:** Collect sufficient process, network, persistence, and file evidence before performing destructive remediation where forensic preservation is required.

---

## Step 3: Remediation via RTR

### Kill a Malicious Process

```text id="6v4s9k"
kill
```

> ⚠️ Identify the correct malicious process before terminating it.

### Delete a Malicious File

```text id="4p3w6e"
rm C:\Temp\malware.exe
```

> ⚠️ Preserve forensic evidence before deletion when the incident requires evidence acquisition.

### Run a Remediation Script

```text id="t7j2f1"
runscript -CloudFile=cleanup_ransomware.ps1
```

### Deploy a File to the Host

Push a file from the Falcon cloud:

```text id="x4e7wq"
put quarantine_tool.exe
```

### Run an Antivirus Full Scan

```text id="n2j6ab"
runscript -CloudFile=av_scan.ps1
```

> 🛡️ **Remediation Principle:** Perform containment and evidence-preservation activities according to the incident-response procedure before making irreversible changes.

---

## Step 4: Post-Containment Verification

After remediation, verify that the threat has been removed before restoring normal network access.

### Verify Malicious Processes

```text id="3b6q9s"
ps | grep malware_name
```

Confirm that malicious processes are no longer running.

### Verify Persistence Removal

Check for remaining persistence mechanisms, including:

* Registry keys
* Scheduled tasks
* Services

### Verify C2 Connections

Use:

```text id="e7k1p2"
netstat
```

Confirm that connections to known C2 IP addresses are no longer present.

### Re-scan with Falcon

Trigger an on-demand scan through the applicable Falcon policy.

### Release Network Containment

Release containment **only after verification**:

**Actions** → **Lift Network Containment**

### Post-Remediation Monitoring

Monitor the host's detections for:

**24–48 hours**

> 🔎 **Verification Requirement:** Do not restore network connectivity until the endpoint has been investigated, remediated, and verified clean.

---

## ⭐ Best Practice Tips

* **Pre-build remediation scripts in Falcon cloud** (**Response** → **Scripts**) — keep approved scripts ready for rapid deployment.
* **RTR custom scripts can run as SYSTEM** — use extreme caution with destructive commands.
* **Use `runscript -Raw` for one-liner PowerShell** — there is no need to pre-stage a script file.
* **Contain FIRST, investigate SECOND** — stop the bleeding before conducting deeper forensics, while preserving required evidence.
* **Never release containment based on assumption** — verify using RTR commands and a clean scan.
