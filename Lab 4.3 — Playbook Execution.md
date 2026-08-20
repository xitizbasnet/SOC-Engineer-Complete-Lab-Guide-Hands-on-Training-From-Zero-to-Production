# Lab 4.3 — Playbook Execution

## Objective

**Follow structured response playbooks for common attack scenarios.**

---

## Step 1: Phishing Email Investigation Playbook

### 1. Initial Alert Handling

**L1: Receive alert** → **Pull email headers** → **Analyse SPF/DKIM/DMARC results**

Review the following:

* Sender domain age
* MX record
* VirusTotal submission

### 2. Extract Email Indicators

Extract all URLs and attachments and submit them to an approved sandbox for analysis.

Use:

```bash
emlAnalyzer -i phishing.eml --header --urls --attachments
```

### 3. Confirmed Phishing Response

If the email is confirmed as phishing:

* Block the sender domain in the email gateway.
* Search for and pull similar emails from other users.
* Notify affected users.
* Reset credentials if credentials were harvested.

---

## Step 2: Ransomware Response Playbook

Follow the response sequence below.

### 🛑 CONTAIN

Immediately isolate the affected host from the network.

Possible methods:

* Disable the network interface (NIC).
* Place the host into NAC quarantine.

### 🔎 IDENTIFY

Determine the ransomware strain.

* Identify the ransomware family.
* Look up available decryptors on **nomoreransom.org**.

### 🔐 PRESERVE

Before performing remediation:

* Take a memory dump.
* Acquire a disk image.

> ⚠️ **Forensic Preservation:** Preserve evidence before remediation activities where operationally feasible.

### 🔭 SCOPE

Identify all affected systems by querying the SIEM for the same IOCs.

Investigate:

* IP addresses
* Domains
* File hashes
* Other indicators associated with the ransomware activity

### 📢 ESCALATE

Immediately notify:

* **CISO**
* **Legal**
* **DR team**

### ♻️ RECOVER

Restore affected systems from the **last known clean backup**.

> 🔐 **Recovery Requirement:** Verify backup integrity before performing the restore.

---

## Step 3: Account Compromise Playbook

### 🔎 DETECT

Look for indicators such as:

* Impossible-travel login
* Login from Tor/VPN
* Abnormal login hours

### ☎️ VERIFY

Contact the user by **phone**, not email.

> ⚠️ **Important:** The user's email account may already be compromised.

### 🛑 CONTAIN

Disable the account in Active Directory:

```powershell
Disable-ADAccount -Identity jdoe
```

### 🔑 RESET

Perform the following actions:

* Force a password reset.
* Revoke all active sessions.
* Revoke all active tokens.

### 🔍 INVESTIGATE

Review all actions taken during the compromise window, including:

* Email forwarding rules
* OAuth grants

### 🧹 REMEDIATE

Remove any persistence left by the attacker, including:

* Inbox rules
* Application permissions

---

## ⭐ Best Practice Tips

* **Playbooks should be tested quarterly** — tabletop exercises keep teams sharp.
* **Automate playbook execution with SOAR** (Splunk SOAR, Palo Alto XSOAR) — reduce MTTR by 70%+.
* **Always have an out-of-band communication channel ready** (Signal/phone) in case email is compromised.
* **Conduct a post-incident review within 72 hours** — document what worked, what did not, and what should be improved.
* **Keep playbooks version-controlled in a wiki/Git repository** — they are living documents.
