# Module 10: Palo Alto Networks

## Panorama, NGFW, Cortex XDR & WildFire Threat Prevention

Palo Alto Networks delivers next-generation firewall (NGFW) technology with **App-ID, User-ID, and Content-ID**. Cortex XDR is a market-leading XDR platform. Panorama provides centralised management across all PA firewalls.

---

# Lab 10.1 — Panorama & NGFW Policy

## Objective

**Configure centralised management and NGFW security policies.**

---

## Step 1: Panorama Setup & Device Registration

### Access Panorama

Navigate to:

```text
https://<Panorama-IP>
```

Log in with the appropriate administrator credentials.

### Organise Firewalls with Device Groups

Navigate to:

**Panorama** → **Device Groups**

Organise firewalls according to location or function, for example:

* `HQ-FW`
* `Branch-FW`

### Add a Managed Device

Navigate to:

**Panorama** → **Managed Devices** → **Add**

Enter:

* Firewall serial number
* Firewall IP address

### Configure the Panorama Server on the PA Firewall

On the Palo Alto firewall CLI:

```text
set deviceconfig system panorama-server <Panorama-IP>
```

### Verify Device Registration

In Panorama, verify that the firewall appears as:

> 🟢 **Connected**

---

## Step 2: Configure Security Policy Rule

### Create a Block-TOR-Traffic Rule

Navigate to:

**Panorama** → **Device Group** → **Security Policy** → **Pre Rules** → **Add**

Configure:

| Setting              | Value               |
| -------------------- | ------------------- |
| **Name**             | `Block-TOR-Traffic` |
| **Source Zone**      | `Trust`             |
| **Destination Zone** | `Untrust`           |
| **Application**      | `tor`               |
| **Action**           | `Deny`              |
| **Logging**          | Log at session end  |

### Create an Allow-Web-Browsing Rule

Add the following rule:

| Setting         | Value                 |
| --------------- | --------------------- |
| **Name**        | `Allow-Web-Browsing`  |
| **Application** | `web-browsing`, `ssl` |
| **Action**      | `Allow`               |

### Attach Security Profiles

Attach a **Security Profiles group** containing:

* Antivirus (AV)
* Vulnerability Protection
* URL Filtering
* WildFire
* Anti-Spyware

### Commit and Push the Configuration

After validating the configuration:

```text
Commit → Push → All Devices
```

> ⚠️ **Change Control:** Validate policy changes before pushing them to production firewalls, particularly when modifying deny or allow rules.

---

## Step 3: User-ID & App-ID Configuration

### Enable User-ID

Navigate to:

**Device** → **User Identification**

Enable **User-ID** on the **Trust** zone.

### Configure Active Directory Integration

Configure the **User-ID Agent** on the domain controller.

The User-ID Agent reads **Security event logs** to associate network activity with users.

### Start the User-ID Agent

On the User-ID Agent server:

```cmd
net start PanUserIDAgent
```

### Verify User Identification

Navigate to:

**Monitor** → **Logs** → **Traffic**

Verify that the **Username** field is populated.

> 🔎 **Validation:** Traffic should display the associated username rather than `unknown`.

### Understand App-ID

**App-ID** identifies applications based on their behaviour rather than relying solely on network ports.

Examples include:

* `ssl`
* `facebook`
* `ssh`

This allows a policy such as:

```text
Allow ssl to Google
```

rather than:

```text
Allow port 443 to all
```

> 💡 **Security Benefit:** Application-aware policies provide more granular control than port-based rules alone.

---

## Step 4: URL Filtering with PAN-DB

### Create a URL Filtering Profile

Navigate to:

**Security Profiles** → **URL Filtering** → **Add Profile**

### Categories to Block

Configure the following categories for blocking:

* **Malware**
* **Phishing**
* **Hacking**
* **Command-and-Control**
* **Unknown**

### Categories to Alert

Configure the following categories for visibility and monitoring:

* **Gaming**
* **Social-networking**
* **Streaming-media**

### Configure Custom URL Categories

Create a custom URL category and add internal domains to the:

```text
safe
```

category.

### Enable SafeSearch

Enable:

> 🔍 **SafeSearch enforcement for all search engines**

---

## ⭐ Best Practice Tips

* **Use Template Stacks in Panorama for consistent device settings** — individual overrides should be rare.
* **Shadow Rules:** Check Panorama for rules that are never hit and clean them up quarterly.
* **App-ID requires applications to be fully identified** — use `application-default` ports to prevent tunnelling.
* **Log all traffic (not just denies)** — you cannot investigate what you don't log.
* **Understand rule evaluation order:**

```text
Pre-Rules (Panorama)
        ↓
Local Rules (FW)
        ↓
Post-Rules (Panorama)
```

> 📌 **Key Concept:** Understanding the relationship between Panorama Pre-Rules, firewall Local Rules, and Panorama Post-Rules is essential when troubleshooting policy behaviour and determining which rule takes effect.
