# Lab 2.2 — Dynamic, Behavioral Analysis

## Objective

**Execute malware in a controlled sandbox and observe behaviour.**

> 🚨 **WARNING — Malware Safety:** Perform dynamic malware analysis only in an isolated, controlled environment. Never execute unknown malware on a production system or shared network.

---

## Step 1: Prepare Monitoring Tools (Before Execution)

Before executing the malware sample, launch and configure the following monitoring tools.

### Process Monitor (Procmon)

1. Launch **Procmon (Process Monitor)**.
2. Set the filter:

```text
Process Name is malware.exe
```

### Wireshark

1. Launch **Wireshark**.
2. Start a capture on the **Host-Only adapter**.

### Regshot

1. Launch **Regshot**.
2. Take the **1st snapshot** of the registry.

### Process Hacker

1. Launch **Process Hacker (advanced Task Manager)**.
2. Monitor newly created processes.

### Establish a Baseline

Before launching the malware, note:

* All currently running processes.
* Open network connections.

> 💡 **Purpose:** Establishing a baseline makes it easier to identify changes caused by the malware during and after execution.

---

## Step 2: Execute Malware & Observe

1. Execute `malware.exe` by double-clicking it, or drag the sample into a controlled sandbox such as **Cuckoo** or **ANY.RUN**.

2. Wait **2–5 minutes** for potential C2 beaconing or payload drops.

### Observe in Procmon

Monitor for:

* File writes
* Registry modifications
* Process spawning

### Observe in Wireshark

Monitor for:

* DNS queries
* HTTP/HTTPS connections
* IRC traffic
* Raw TCP connections

### Observe in Process Hacker

Monitor for:

* Injected DLLs
* Child processes
* Memory allocations

---

## Step 3: Post-Execution Analysis

### Regshot

1. Take the **2nd snapshot**.
2. Compare the first and second snapshots.
3. Review added or modified registry keys.

### Procmon

1. Save the Procmon log.
2. Apply the following filter:

```text
Operation=WriteFile
```

3. Use the filter to identify potentially dropped files.

### Identify Dropped Payloads

Run:

```cmd
dir /s /b %APPDATA% %TEMP% %SYSTEMROOT%\Temp
```

> 💡 **Purpose:** Identify files potentially dropped into common temporary or application data directories.

### Wireshark

1. Identify suspicious network connections.
2. Use **Follow TCP Stream** on suspicious connections.
3. Extract the C2 protocol where applicable.

### Document Key Findings

Record:

* **Mutex names** — used to prevent double execution.
* **Persistence keys**
* **C2 URL/IP**

---

## Step 4: Submit to Online Sandbox

Upload the sample to an approved online sandbox:

* **ANY.RUN**
* **Joe Sandbox**
* **Triage (tria.ge)**

### Review the Automated Report

Review:

* Process tree
* Network IOCs
* MITRE mapping

### Extract Network IOCs

Extract:

* C2 IP addresses
* C2 domains

> 🚨 **Action:** Block identified C2 IPs/domains in the firewall or proxy immediately, subject to your organization's incident-response procedures.

### Extract YARA Rules

Extract YARA rules from the sandbox output for detection in **AV/EDR**.

---

## ⭐ Best Practice Tips

* **Use FakeNet-NG** to simulate network services — malware will 'think' it reached its C2 and reveal more behaviour.
* **Configure INetSim** on a separate VM to provide fake DNS/HTTP/SMTP responses.
* **Take memory dumps at key moments** (right after process injection) for later analysis.
* **Use API Monitor** to log all Windows API calls — reveals obfuscated behaviour.
* **Check for anti-analysis tricks:** `IsDebuggerPresent`, CPUID checks, and sleep calls > 2 min.
