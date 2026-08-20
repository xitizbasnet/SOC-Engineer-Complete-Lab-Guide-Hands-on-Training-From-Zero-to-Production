# Module 6: PAM — Privileged Access Management

## CyberArk Vault, Session Recording & JIT Access

PAM solutions protect and monitor privileged accounts — the highest-value targets for attackers. CyberArk is the industry-leading PAM platform, providing password vaulting, session isolation, and just-in-time access.

---

# Lab 6.1 — CyberArk PAM Setup & Vaulting

## Objective

**Configure CyberArk vault and onboard privileged accounts.**

---

## Step 1: CyberArk Component Overview

CyberArk PAM consists of several core components that work together to secure privileged access.

| Component                            | Description                                         |
| ------------------------------------ | --------------------------------------------------- |
| **Digital Vault (PAS)**              | Encrypted storage for privileged credentials        |
| **PVWA (Password Vault Web Access)** | Web UI for users to request/access credentials      |
| **CPM (Central Policy Manager)**     | Automatically rotates passwords according to policy |
| **PSM (Privileged Session Manager)** | Session proxy and recording for RDP/SSH             |
| **PSMP (SSH Proxy)**                 | Routes SSH sessions through the PAM gateway         |

> 💡 **Architecture Note:** The components collectively provide credential protection, controlled access, password rotation, session monitoring, and session recording.

---

## Step 2: Log in to PVWA & Create Safe

### 1. Access PVWA

Navigate to:

```text
https:///PasswordVault
```

2. Log in using **PAM administrator credentials**.

> 🔐 **Security Requirement:** Use MFA for administrator accounts.

### 2. Create a Safe

Navigate to:

**Policies** → **Safes** → **Add Safe**

Configure:

```text
Name: WinAdmins_Prod
```

3. Assign:

   * **CPM**
   * **Owner:** PAM Admin
   * **Members:** IT operations team

4. Configure retention:

   * **Day retention for versions:** e.g., `90 days`
   * **Required number of days:** `0` for no expiry

> 🛡️ **Access Control:** Safe membership should be limited to users and services that require privileged access.

---

## Step 3: Onboard Privileged Account

1. Navigate to:

   **Accounts** → **Add Account**

2. Configure the account:

   | Setting            | Value                             |
   | ------------------ | --------------------------------- |
   | **Platform**       | `WindowsServerLocalAdministrator` |
   | **Target Address** | `10.1.1.50`                       |
   | **Username**       | `Administrator`                   |
   | **Safe**           | `WinAdmins_Prod`                  |

3. Save the account.

4. The **CPM** automatically discovers and changes the password immediately on the first cycle.

5. Verify that the account displays:

   **Green — `Managed`**

6. Verify the **last CPM change timestamp**.

> 🔎 **Validation:** A `Managed` status confirms that the account is under CPM management.

---

## Step 4: Set Password Policy

1. Navigate to:

   **Administration** → **Platform Management**

2. Select the appropriate platform.

3. Select:

   **Edit**

### Configure Password Settings

Set:

* **Complexity:** `20 chars`
* **Expiry:** `24 hours (break-glass)` or `30 days`
* **Automatic Password Management:** Enabled

### Production Administrative Accounts

Enable:

**Require Dual Control (4-eyes approval)**

This requires a second approval before privileged production access is granted.

### Just-In-Time Access

Enable:

**Just-In-Time access for elevated accounts**

> 🕒 **JIT Principle:** Just-In-Time access limits privileged access to the period in which elevated permissions are actually required.

---

## ⭐ Best Practice Tips

* **All privileged accounts should be vaulted** — zero standing access is the end goal.
* **Separate safes by tier:**

  * **Tier 0:** Domain Controllers
  * **Tier 1:** Servers
  * **Tier 2:** Workstations
* **Enforce dual-control approval for Tier 0 accounts** — requires a second approval before password checkout.
* **Review Safe membership quarterly** and remove stale members immediately.
* **Monitor accounts that are checked out but have no sessions initiated** — this may indicate potential credential theft.
