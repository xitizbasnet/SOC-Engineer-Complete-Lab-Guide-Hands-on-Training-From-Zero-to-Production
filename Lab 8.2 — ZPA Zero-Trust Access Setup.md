# Lab 8.2 — ZPA Zero-Trust Access Setup

## Objective

**Replace VPN with identity-based application access via ZPA.**

---

## Step 1: ZPA Architecture Components

ZPA provides identity-based access to private applications without exposing internal applications directly to the internet.

| Component                  | Description                                                                                                  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **App Connector**          | Deployed in a data centre or cloud environment; establishes an outbound-only connection to the Zscaler cloud |
| **Client Connector (ZCC)** | Installed on the user endpoint; provides the agent required for ZPA access                                   |
| **ZPA Admin Portal**       | `admin.private.zscaler.com` — used to configure application segments and policies                            |

> 🔐 **Zero-Trust Architecture:** No inbound firewall ports are needed because connections are broker-mediated.

---

## Step 2: Deploy App Connector

1. In the ZPA Admin Portal, navigate to:

   **Connectors** → **Add Connector**

2. Download the appropriate deployment image:

   * **OVA** for supported on-premises virtual environments
   * **Docker image** for container-based deployment

### Docker Deployment

Use:

```cmd
docker run -e ZPA_CONNECTOR_KEY=<connector_key> zscaler/app-connector:latest
```

3. The connector registers with the Zscaler cloud.

4. Verify that the connector appears as:

   **Green / Connected**

   in the ZPA console.

### High Availability

Deploy **2 or more connectors per location** for high availability using an active-active configuration.

> 🛡️ **Availability Requirement:** Multiple connectors help maintain application access if an individual connector becomes unavailable.

---

## Step 3: Create App Segment & Access Policy

### Create the Application Segment

1. Navigate to:

   **ZPA Admin** → **Application Segments** → **Add Application Segment**

2. Configure:

   | Setting             | Value                   |
   | ------------------- | ----------------------- |
   | **Name**            | `Internal-JIRA`         |
   | **Domain**          | `jira.company.internal` |
   | **Port**            | `443`                   |
   | **Connector Group** | `DC-Mumbai-Connectors`  |

### Configure Access Policy

1. Navigate to:

   **Policy** → **Access Policy** → **Add Rule**

2. Configure the conditions:

   ```text
   User = Engineering Group (from IdP)
   AND
   Device = Compliant
   ```

3. Set:

   ```text
   Action: Allow
   App Segment: Internal-JIRA
   ```

> 🔐 **Policy Principle:** Access should be granted based on verified identity and device posture rather than network location.

---

## Step 4: Install & Test ZCC Client

### Install Zscaler Client Connector

Users install **Zscaler Client Connector** from the company portal.

### Authenticate

1. Launch Zscaler Client Connector.
2. Sign in using **SSO**.

Supported identity providers include:

* Azure AD
* Okta

### Test Application Access

Browse to:

```text
jira.company.internal
```

ZPA brokers the connection transparently.

### Verify User Activity

In the ZPA Admin Portal, navigate to:

**Insights** → **User Activity**

Confirm the successful tunnel/session.

### Verify Access Restrictions

Confirm that the user **cannot access other internal resources** that are not included in their assigned App Segment policy.

> 🧪 **Validation Objective:** Successful access to the authorised application combined with denial of unauthorised internal resources demonstrates application-level least-privilege enforcement.

---

## ⭐ Best Practice Tips

* **ZPA provides micro-segmentation** — users access only what they need, and nothing else, supporting least-privilege network access.
* **Enable Continuous Trust Evaluation** — re-verify user and device posture on every session.
* **Use Posture Profiles** — allow access only from managed, compliant devices.
* **Monitor ZPA logs** for:

  * Access denied events
  * Unusual application access patterns
  * Off-hours access
* **Decommission VPN once all applications are moved to ZPA** — do not run both long-term.
