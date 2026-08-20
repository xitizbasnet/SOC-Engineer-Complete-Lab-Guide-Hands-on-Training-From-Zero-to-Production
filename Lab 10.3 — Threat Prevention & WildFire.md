# Lab 10.3 — Threat Prevention & WildFire

## Objective

**Configure IPS, anti-spyware, and cloud sandboxing.**

---

## Step 1: Vulnerability Protection (IPS) Profile

### Create the Vulnerability Protection Profile

Navigate to:

**Security Profiles** → **Vulnerability Protection** → **Add Profile**

### Configure Severity-Based Actions

Configure the following rules:

| Severity            | Action   |
| ------------------- | -------- |
| **Critical / High** | Block-IP |
| **Medium**          | Alert    |
| **Low / Info**      | Allow    |

For **Critical/High** events, configure:

```text
Block-IP: 300 seconds
```

### Enable Brute-Force Protection

Enable:

> **Brute Force Protection**

Configure the profile to track and block brute-force sources.

### Apply the Profile

Apply the Vulnerability Protection profile to:

* All internet-facing security policies
* Inbound traffic policies
* Outbound traffic policies

> 🛡️ **Security Objective:** Applying IPS controls to both inbound and outbound policies helps protect internet-facing services while also detecting compromised internal systems communicating externally.

---

## Step 2: Anti-Spyware Profile

### Create the Anti-Spyware Profile

Navigate to:

**Security Profiles** → **Anti-Spyware** → **Add**

### Enable DNS Signatures

Enable:

> **DNS Signatures**

Configure known C2 domains to be redirected to an internal DNS sinkhole.

### Configure DNS Sinkhole

Use:

```text
DNS Sinkhole IP:
100.100.100.100
```

> 🚨 **Detection Indicator:** Any host resolving a malicious domain to `100.100.100.100` is a potential indicator of malware infection.

### Configure C2 Callback Signatures

Configure:

* **High/Critical** → Block
* **Medium** → Alert

### Apply to Outbound Traffic

Apply the Anti-Spyware profile to outbound traffic policies.

> 🔎 **Detection Objective:** Outbound inspection helps identify compromised endpoints exhibiting C2 beaconing or callback behaviour.

---

## Step 3: WildFire Cloud Sandbox

### Configure WildFire

Navigate to:

**Device** → **Setup** → **WildFire**

Set the subscription API key obtained from the customer support portal.

### Configure WildFire Analysis

Navigate to:

**Security Profiles** → **WildFire Analysis**

Use the **Default** profile and configure it to forward all applicable file types.

### File Types to Forward

Configure WildFire analysis for:

* PE
* PDF
* Office
* JAR
* APK
* Scripts

### Apply the WildFire Profile

Apply the WildFire profile to **all security policies**.

### WildFire Analysis Workflow

WildFire analyses submitted files in a cloud sandbox:

```text
File
  ↓
WildFire Cloud Sandbox
  ↓
Behavioural / Threat Analysis
  ↓
Verdict
  ↓
Automatic Signature Update
```

The provided workflow specifies a verdict target of **within 5 minutes**.

### Monitor WildFire Submissions

Navigate to:

**Monitor** → **WildFire Submissions**

Review submitted files and their resulting verdicts.

---

## Step 4: DNS Security & Anti-Phishing

### Enable DNS Security

Navigate to:

**Security Profiles** → **Anti-Spyware** → **DNS Policies**

Enable:

> **DNS Security**

### Configure DNS Security Categories

Configure the following categories for sinkholing:

* **C2**
* **Dynamic DNS**
* **Malware**

### Enable Passive DNS Monitoring

Enable:

> **Passive DNS monitoring**

Use this capability to improve visibility into internal DNS activity.

### Configure URL Filtering

Enable:

> **Phishing real-time analysis**

Use cloud-based lookup rather than relying exclusively on the category database.

### Enable PAN-DB Credential Phishing Detection

Enable:

> **PAN-DB credential phishing detection**

Configure the control to block credential submission to phishing sites.

> 🎯 **Security Objective:** Combining DNS security, URL filtering, and credential-phishing detection provides layered protection against phishing and C2 activity.

---

## ⭐ Best Practice Tips

* **WildFire is most effective when forwarding unknown files** — not just known-malicious files — trust the cloud verdict.
* **DNS sinkhole is one of the highest-value, lowest-friction security controls available on PA** — always enable it.
* **Review WildFire verdicts weekly** — new malware families often show up here before AV vendors.
* **IPS false positives on business-critical servers cause outages** — tune exclusions carefully for production apps.
* **Use Security Policy Optimizer in Panorama** to right-size policies based on actual traffic patterns.
