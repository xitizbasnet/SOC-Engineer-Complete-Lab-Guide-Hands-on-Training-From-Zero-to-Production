# Module 8: Zscaler

## ZIA Internet Access + ZPA Private Access — Zero Trust Cloud Security

Zscaler is a cloud-native security platform providing Secure Web Gateway (SWG), CASB, DLP, and Zero Trust Network Access (ZTNA). ZIA inspects all internet traffic; ZPA replaces VPN with identity-based private access.

---

# Lab 8.1 — ZIA Policy Configuration

## Objective

**Configure URL filtering, SSL inspection, and firewall policies in ZIA.**

---

## Step 1: ZIA Admin Console Overview

### Access the ZIA Admin Console

Log in to:

```text id="6k3x5b"
admin.zscaler.com
```

Or use the tenant-specific URL.

### Key Menus

| Menu                                 | Purpose                                      |
| ------------------------------------ | -------------------------------------------- |
| **Policy → URL & Cloud App Control** | Configure URL and cloud application policies |
| **Policy → Firewall**                | Configure cloud firewall filtering policies  |
| **Insights → Reports**               | Review security and traffic reports          |

### Configure Locations

Configure traffic forwarding using:

* On-premises **GRE/IPSec tunnels**
* **Zscaler Client Connector (ZCC)** for remote users

### Verify Subscription Modules

Navigate to:

**Administration** → **Company Profile**

Verify the available subscription modules.

> 🧭 **Administration Tip:** Confirm that the required ZIA capabilities are included in the tenant subscription before configuring policies that depend on them.

---

## Step 2: Configure URL Filtering Policy

1. Navigate to:

   **Policy** → **URL & Cloud App Control** → **Add Rule**

2. Configure the rule:

   ```text
   Rule Name: Block Malicious Categories
   ```

3. Select the following categories:

   * **Malware Sites**
   * **Phishing**
   * **Botnets**
   * **Spyware/Adware**

4. Set the action:

   **Block + Notify**

   This displays a block page to the user.

5. Configure the scope:

   * All users, or
   * Specific departments/groups synchronized from AD

6. Set the rule order.

> ⚠️ **Rule Ordering:** Place block rules **above allow rules** because ZIA rules are evaluated top-down.

---

## Step 3: Enable SSL Inspection

1. Navigate to:

   **Policy** → **SSL Inspection** → **Add SSL Inspection Rule**

> ⚠️ **Compliance Requirement:** SSL inspection decrypts HTTPS traffic and requires appropriate legal, HR, and compliance sign-off.

### Configure Inspection Exceptions

Exclude the following from inspection where required:

* Banking sites
* Healthcare portals
* Certificate-pinned applications

### Deploy Zscaler Root CA

Deploy the **Zscaler Root CA** to all applicable endpoints through:

* **GPO**
* **MDM**

This allows browsers to trust certificates generated during HTTPS interception.

### Test SSL Inspection

1. Browse to an HTTPS website.
2. Open the browser certificate information.
3. Verify that the Zscaler certificate appears in the certificate chain.

> 🔐 **Validation:** Confirm that SSL inspection is functioning as intended without disrupting approved applications and services.

---

## Step 4: Cloud Firewall Rules

1. Navigate to:

   **Policy** → **Firewall** → **Add Firewall Filtering Rule**

### Block Known C2 Ports

Block outbound:

* **TCP**
* **UDP**

to known C2 ports, including:

```text
4444
8080
```

when these ports are used by non-standard servers.

### Block Tor Exit Nodes

Configure blocking for **Tor exit nodes**.

> 🛡️ Zscaler maintains the applicable Tor exit-node list automatically.

### Detect Newly Registered Domains

Configure alerts for DNS requests to domains that were registered:

```text
< 30 days ago
```

### Enable Cloud IPS

Enable **Cloud IPS (Intrusion Prevention)** using:

**Alert + Block**

for known signatures.

---

## ⭐ Best Practice Tips

* **Always activate policy changes during a maintenance window** — ZIA rules go live instantly.
* **Use the Policy Simulator in ZIA** to test rules before activation.
* **Monitor NSS (Nanolog Streaming Service) logs** and forward them to Splunk/SIEM for correlation.
* **Regularly review Shadow IT reports in ZIA** to discover unapproved SaaS applications in use.
* **Review the SSL inspection bypass list quarterly** — do not blindly bypass entire categories.
