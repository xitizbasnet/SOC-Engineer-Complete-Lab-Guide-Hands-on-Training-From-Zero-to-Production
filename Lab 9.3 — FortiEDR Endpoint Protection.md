# Lab 9.3 — FortiEDR Endpoint Protection

## Objective

**Deploy FortiEDR and investigate endpoint detections.**

---

## Step 1: FortiEDR Deployment

### Access the FortiEDR Console

Use either the cloud-based console or an on-premises Central Manager:

```text
Cloud:
https://fortiEDR.fortiguard.net
```

> 📝 **Note:** Use the appropriate console URL for your FortiEDR deployment.

### Download and Deploy the FortiEDR Collector

Download the **FortiEDR collector** for the applicable operating system:

* Windows
* Linux
* Mac

For enterprise-scale deployment, use:

* **GPO**
* **SCCM**
* **Ansible**

### Windows Silent Installation

Run:

```cmd
msiexec /i FortiEDRInstaller.msi /quiet REGISTRATION_CODE=<registration_code>
```

> 🔐 **Security Note:** Protect the registration code and avoid exposing it in publicly accessible scripts or documentation.

### Verify Endpoint Registration

After deployment:

1. Open the FortiEDR console.
2. Navigate to the endpoint/host inventory.
3. Verify that hosts appear as:

   * **Registered**
   * **Connected (green)**

---

## Step 2: Investigate a FortiEDR Event

### Review Classified Events

Navigate to:

**FortiEDR** → **Event Viewer**

Review events classified as:

* **Ransomware**
* **Exfiltration**
* **Malicious**
* **Suspicious**

### Examine the Process Chain

1. Select the relevant event.

2. Navigate to:

   **Forensics**

3. Review the full process chain, including:

   * File context
   * Registry context
   * Network context

> 🔎 **Investigation Tip:** Review the complete process chain rather than relying solely on the event classification. Parent-child relationships and associated network activity can provide important context.

### Perform Threat Hunting

Navigate to:

**Threat Hunting**

Run queries across all endpoints using relevant indicators, such as:

* File hash
* IP address
* Domain

### Isolate a Compromised Endpoint

Navigate to:

**Response**

Use the endpoint isolation capability to isolate the endpoint from the network while retaining its **FortiEDR cloud connection**.

### Remediate the Malicious Process

Perform the appropriate response actions:

1. **Terminate the malicious process.**
2. **Block the threat at the EDR signature level.**

> ⚠️ **Caution:** Validate the detection and its business impact before terminating processes or applying blocking actions to production endpoints.

---

## Step 3: Configure Prevention Policies

### Assign Security Policies

Navigate to:

**FortiEDR** → **Security Policies**

Assign appropriate policies to the relevant device groups.

### Begin in Simulation Mode

Start with:

**Simulation Mode**

After baseline validation, transition to:

**Production Mode**

Use a baseline validation period of **2 weeks** before enforcing blocking policies.

### Enable Endpoint Protections

Enable:

* **Ransomware Protection**
* **Pre-Execution Detection**
* **Credential Theft Prevention**

### Configure Custom Exceptions

Create exceptions for known-good internal tools using:

* File hash
* Signer

> 🛡️ **Least-Privilege Principle:** Keep exceptions narrowly scoped and review them periodically to prevent allowlists from becoming an avenue for bypassing endpoint protection.

### Integrate with FortiSandbox

Integrate FortiEDR with **FortiSandbox** so that:

```text
Unknown file
    ↓
Automated sandbox detonation
    ↓
Behavioural analysis
    ↓
Detection / classification
    ↓
EDR response
```

---

## ⭐ Best Practice Tips

* **FortiEDR's kernel-level driver gives visibility that userspace AV misses** — protect it from tampering.
* **Use Simulation Mode for a minimum of 2 weeks before blocking** — prevents business disruption.
* **FortiEDR + FortiGate integration enables automatic firewall block when EDR detects a threat.**
* **Check FortiEDR coverage map (enrolled vs. expected endpoints) weekly** — gaps are risk.
* **Threat Hunting custom queries save significant investigation time** — build a library of reusable queries.
