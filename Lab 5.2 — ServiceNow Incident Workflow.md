# Lab 5.2 — ServiceNow Incident Workflow

## Objective

**Create, manage and close security incidents in ServiceNow.**

---

## Step 1: Create Security Incident Record

1. Navigate to:

   **ServiceNow** → **Security Incident Response** → **Create New**

2. Complete the required incident information:

   * **Short Description**
   * **Category**

     * Malware
     * Phishing
     * Unauthorised Access
   * **Priority**

3. Assign the incident to:

   **SOC Team Queue** → **Initial assignment to L1 analyst**

4. Set the following fields:

   * **State** = `New`
   * **Impact**

     * `1` = Enterprise
     * `2` = Department
     * `3` = User
   * **Urgency**

5. The **Priority** is automatically calculated using the **Impact × Urgency** matrix.

> 💡 **Tip:** Ensure Impact and Urgency accurately represent the business and operational effect of the security incident.

---

## Step 2: Work Notes & Evidence Attachment

### Investigation Documentation

* Record all investigation notes in **Work Notes (internal)** — not customer-visible.
* Update the **Activity Log** with each action taken and its timestamp.
* Attach relevant investigation evidence, including:

  * Screenshots
  * PCAP excerpts
  * Malware hash report
  * IOC list

> 🔐 **Sensitive Information:** Never put sensitive details, such as passwords or PII, in comments visible to end users.

### Incident State Progression

Follow the standard state progression:

```text
New → In Progress → Pending → Resolved
```

> 📝 **Documentation Requirement:** Maintain a complete chronological record of investigation and response actions throughout the incident lifecycle.

---

## Step 3: Escalation & SLA Management

### SLA Monitoring

If an SLA breach is approaching:

* ServiceNow automatically sends an email notification.
* Manager escalation is triggered.

### Manual Escalation

When additional expertise is required:

* Change the **Assignment Group** to the appropriate **L2/L3 SOC Team**.

### End-User Communication

For **P1/P2** incidents:

* Update the **Caller (end user)** with the incident status every **2 hours**.

### Related Incidents

Use the **Related Records** tab to link related incidents.

> 🔎 **Purpose:** Identify whether multiple individual events are part of the same campaign.

### Major Incident

For **P1** incidents:

* Enable the **Major Incident** flag.
* This triggers the separate **major incident process**.

---

## ⭐ Best Practice Tips

* **Use the ServiceNow SIR (Security Incident Response) module** — it is purpose-built for SOC workflows.
* **Automate ticket creation from SIEM alerts** via ServiceNow MID Server integration.
* **Keep ticket closure criteria clear:**

  * All IOCs blocked.
  * Host re-imaged.
  * User notified.
  * Root cause documented.
* **Review recurring incident categories weekly** — repeated incidents indicate an unresolved root cause.
* **Export incident metrics monthly**, including:

  * MTTD
  * MTTR
  * Incidents by category
  * Analyst performance
