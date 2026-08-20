# Lab 9.2 — FortiAnalyzer & FortiSIEM

## Objective

**Centralise logs and build security analytics with Fortinet tools.**

---

## Step 1: FortiAnalyzer Setup

### Access FortiAnalyzer

Navigate to:

```text
https://<FortiAnalyzer-IP>
```

> 📝 Replace `<FortiAnalyzer-IP>` with the address of the FortiAnalyzer instance in your environment.

### Add FortiGate Device

1. Navigate to:

   **Device Manager** → **Add Device**

2. Select:

   **FortiGate**

3. Authorise the connection from the FortiGate.

### Configure Log Storage

Use **ADOM (Administrative Domain)** to organise log storage:

* Per device group, or
* Per organisation

> 🗂️ **Organisation Tip:** ADOM structure should align with the organisation's operational, business-unit, or customer segmentation requirements.

### Run Compliance Reports

Navigate to:

**Reports**

Run the applicable built-in compliance reports, including:

* **PCI-DSS**
* **ISO 27001**
* **GDPR**

### Configure Automated Response

Configure a playbook with automated response triggers based on relevant log events.

---

## Step 2: Create Custom Log Reports

1. Navigate to:

   **FortiAnalyzer** → **Reports** → **Create New** → **Log Based Report**

2. Add the following charts:

   * **Top Threats by Source IP**
   * **Failed Logins**
   * **Top Web Categories Blocked**

### Schedule Report Delivery

Configure:

```text
Schedule: Daily
Recipient: soc-team@company.com
Output Format: PDF
```

Include the **company logo** in the report.

> 📊 **Reporting Note:** FortiAnalyzer stores logs and runs reports without impacting FortiGate performance.

---

## Step 3: FortiSIEM Event Correlation

FortiSIEM can ingest and correlate security telemetry from multiple sources, including:

* FortiGate logs
* Windows events
* Linux syslog
* Cloud logs

### Review Built-In Correlation Rules

Navigate to:

**Analytics** → **Rules** → **Browse**

Review the built-in rules.

> 📌 **Reference:** The environment includes **500+ predefined rules**.

### Create a Custom Brute-Force Rule

Example detection logic:

```text
5+ failed logins
FROM the same IP
WITHIN 1 minute
→ Alert
```

### Create an Incident Workflow

Configure the workflow:

```text
Rule fires
    ↓
Incident created
    ↓
Assigned to SOC queue
```

### Integrate with FortiSOAR

Integrate **FortiSIEM** with **FortiSOAR** for automated playbook execution.

> 🤖 **Automation Objective:** Use correlation rules to identify suspicious activity and trigger consistent response workflows.

---

## ⭐ Best Practice Tips

* **FortiAnalyzer log compression ratios are excellent** — plan **1 TB storage per 5 Gbps of log traffic**.
* **Use ADOM per customer/BU in MSSP environments** for log isolation.
* **FortiSIEM Performance Analytics (PA)** provides network behaviour baselines and is powerful for anomaly detection.
* **Tune FortiSIEM correlation rules quarterly** — untuned rules generate too many false positives.
* **Export FortiSIEM incidents to ServiceNow/JIRA via REST API** for unified ticketing.
