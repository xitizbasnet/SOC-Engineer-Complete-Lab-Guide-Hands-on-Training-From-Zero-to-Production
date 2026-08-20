# Lab 6.3 — Privilege Elevation Policies

## Objective

**Configure EPM and endpoint least-privilege with CyberArk.**

---

## Step 1: Endpoint Privilege Manager (EPM) Overview

**Endpoint Privilege Manager (EPM)** enforces the principle of least privilege on endpoints.

Key capabilities include:

* EPM enforces least privilege on endpoints — users run as **standard users**.
* Applications requiring administrative privileges can be elevated through policy using a **whitelist approach**.
* Eliminates the need for local administrator rights on workstations.
* Blocks malware that relies on administrative privileges to execute or persist.

> 🛡️ **Least-Privilege Principle:** Users should receive only the privileges required to perform their authorised tasks.

---

## Step 2: Create Application Policy

1. In the **EPM Console**, navigate to:

   **Policies** → **Add Policy** → **Type: Elevation**

2. Define the target application.

   Applications can be identified by:

   * Hash
   * Publisher certificate
   * Path

3. Configure the action:

   **Run Elevated**

   This grants an administrative token only for the specified application.

4. Enable justification:

   **Require user to enter reason**

   > 📝 This creates an audit trail documenting why elevated execution was requested.

5. Define the policy scope:

   * **All Workstations** group, or
   * A specific **OU**

> 🔐 **Policy Design:** Limit elevation policies to explicitly approved applications and appropriate endpoint groups.

---

## Step 3: Block Unknown Applications

1. In EPM, navigate to:

   **Policies** → **Add Restriction Policy**

2. Configure the action:

   **Block and Alert** on unsigned or unknown executables.

3. Define exceptions using the approved software catalog:

   * Hash
   * Publisher-signed software

### Monitor Blocked Events

Navigate to:

**EPM Dashboard** → **Blocked Events**

Review blocked events weekly to identify legitimate applications that should be added to the allowlist.

> 🚨 **Security Objective:** This blocks most commodity malware that relies on auto-run of unsigned binaries.

---

## ⭐ Best Practice Tips

* **Remove local administrator rights from all workstations** — this single action stops 94% of critical vulnerabilities (per Microsoft).
* **Use Application Discovery mode first** — gather **2–4 weeks** of data before enforcing block policies.
* **Certificate-based elevation is more robust than hash-based elevation** — it survives application updates.
* **Audit all elevation events** — frequent elevation of the same application is a candidate for a permanent policy.
* **Combine EPM with MFA for elevation requests** — this helps prevent attackers from abusing elevation policies.
