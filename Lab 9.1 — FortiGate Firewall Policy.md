# Module 9: Fortinet

## FortiGate NGFW, FortiAnalyzer, FortiSIEM & FortiEDR

Fortinet's Security Fabric is an integrated security architecture spanning firewall, endpoint, SIEM, SOAR, and cloud. FortiGate is the flagship NGFW used in thousands of enterprise networks globally.

---

# Lab 9.1 — FortiGate Firewall Policy

## Objective

**Configure NGFW policies, NAT, and security profiles.**

---

## Step 1: FortiGate Initial Setup

### Connect to FortiGate

Access the FortiGate management interface at:

```text id="s3n8j0"
https://192.168.1.99
```

> 🌐 **Default Management IP:** `192.168.1.99`

### Default Credentials

Use:

```text id="0d0c5d"
Username: admin
Password: (blank)
```

> 🔐 **Security Requirement:** Set a strong administrator password immediately after the initial login.

### Run the Setup Wizard

Configure:

* **WAN:** `port1`
* **LAN:** `port2`
* **DNS**
* **NTP**

### Activate Licensing

Activate:

* **FortiCare**
* Security subscriptions, including:

  * IPS
  * Antivirus
  * Web Filter
  * Sandbox

### Update Firmware

Navigate to:

**System** → **Firmware**

Use either:

* **Upload** a firmware package, or
* **FortiGuard auto-update**

> ⚠️ **Change Management:** Perform firmware updates according to the organisation's approved maintenance and change-management process.

---

## Step 2: Create Firewall Policy

1. Navigate to:

   **Policy & Objects** → **Firewall Policy** → **Create New**

2. Configure the policy:

   | Setting                | Value                     |
   | ---------------------- | ------------------------- |
   | **Name**               | `LAN_to_WAN_Web`          |
   | **Incoming Interface** | `port2 (LAN)`             |
   | **Outgoing Interface** | `port1 (WAN)`             |
   | **Source**             | All, or a specific subnet |
   | **Destination**        | All                       |
   | **Schedule**           | Always                    |
   | **Service**            | HTTPS, HTTP, DNS          |
   | **Action**             | ACCEPT                    |

3. Enable **NAT**.

4. Configure:

   **Use Outgoing Interface Address**

5. Assign the required security profiles:

   * Antivirus (AV)
   * Web Filter
   * IPS
   * Application Control
   * SSL Inspection

> 🛡️ **Policy Principle:** Restrict source, destination, services, and security profiles to the minimum required for the approved business use case.

---

## Step 3: Configure Security Profiles

### Antivirus

Navigate to:

**Security Profiles** → **Antivirus** → **Create**

Enable virus scanning for:

* HTTP
* SMTP
* FTP

### Web Filter

Navigate to:

**Security Profiles** → **Web Filter** → **Create**

Configure FortiGuard categories to block:

* Malicious sites
* Adult content

### IPS

Navigate to:

**Security Profiles** → **IPS**

Configure:

**Signature: All**

Set actions:

| Severity            | Action |
| ------------------- | ------ |
| **High / Critical** | Block  |
| **Medium**          | Alert  |

### Application Control

Navigate to:

**Security Profiles** → **Application Control**

Block:

* P2P
* Proxy applications
* Anonymizers

### SSL/SSH Inspection

Navigate to:

**Security Profiles** → **SSL/SSH Inspection** → **Create**

Configure:

**Full inspection**

Deploy the required CA certificate to applicable endpoints.

> 🔐 **SSL Inspection Note:** Ensure certificate deployment, privacy requirements, and applicable organisational policies are addressed before enabling full inspection.

---

## Step 4: Configure Logging

### Enable FortiAnalyzer Logging

Navigate to:

**Log & Report** → **Log Settings**

Enable:

**Log to FortiAnalyzer**

Enter the **FortiAnalyzer IP address**.

Verify connectivity and select:

**Apply**

### Enable Traffic and Security Logging

Enable **All Traffic** logging for security policies.

> ⚠️ **Storage Consideration:** All Traffic logging generates high log volumes. Ensure sufficient storage capacity and retention planning.

Enable logging for:

* **Forward Traffic**
* **Security Events**
* **System Events**

### CLI Configuration

Use:

```text
config log fortianalyzer setting
    set status enable
    set server <FortiAnalyzer-IP>
```

> 📝 Replace `<FortiAnalyzer-IP>` with the address of the FortiAnalyzer instance used by the environment.

---

## ⭐ Best Practice Tips

* **Always test new firewall policies in Monitor mode for 24–48 hours** before switching to Block.
* **Use policy comments to document business justification** — this is invaluable for audits.
* **Apply Least Privilege:** Create specific service objects rather than using `ALL` services in policies.
* **Review unused policies monthly** — disable or delete any policy without traffic in **90+ days**.
* **Enable policy hit count** to identify unused rules:

  **Policy** → **Enable Hit Count** column.
