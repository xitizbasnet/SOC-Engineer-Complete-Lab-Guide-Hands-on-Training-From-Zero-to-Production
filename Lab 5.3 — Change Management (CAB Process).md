# Lab 5.3 — Change Management (CAB Process)

## Objective

**Submit and manage security changes through the Change Advisory Board (CAB).**

---

## Step 1: Types of Change

Changes are classified according to their risk, complexity, urgency, and approval requirements.

### Standard Change

A **Standard Change** is:

* Pre-approved.
* Low-risk.
* Routine.

**Example:**

* AV signature update.

> 💡 **CAB Requirement:** No CAB approval is needed for an approved Standard Change.

### Normal Change

A **Normal Change**:

* Requires CAB approval.
* Must be scheduled within an approved maintenance window.

### Emergency Change

An **Emergency Change** is:

* Urgent.
* Subject to an abbreviated CAB review of **< 4 hours**.
* Required to undergo a **post-implementation review**.

### Security Change Classification

The following changes are classified as **Normal/Emergency**:

* All security patch deployments.
* Firewall rule changes.
* Access changes.

---

## Step 2: Submit Change Request (CR) in ServiceNow

1. Navigate to:

   **ITSM** → **Change** → **Create New**

2. Set:

   **Type:** `Normal`

3. Complete the change request with:

   * **Short Description**
   * **Justification**
   * **Risk Assessment**
   * **Implementation Plan**

### Back-out Plan

Document a **Back-out Plan** describing what must be done if the change fails.

> ⚠️ **Requirement:** A documented rollback procedure is mandatory before submitting the change for approval.

### Testing Evidence

Attach evidence demonstrating that the change was tested successfully in the test environment.

**Example:**

* Screenshot of the test environment result.

### Maintenance Window

Specify:

* Date
* Time
* Stakeholder agreement

### CAB Submission

Assign the change request to **CAB for review**.

> 📅 **Scheduling Requirement:** Submit the change request a minimum of **3 business days before the maintenance window**.

---

## Step 3: CAB Review & Approval

During the review, CAB evaluates:

* **Technical risk**
* **Business impact**
* **Resource availability**
* **Rollback plan**

### Approval Outcomes

#### ✅ Approve

If approved:

* The change proceeds during the approved maintenance window.
* Change state becomes:

```text
Scheduled
```

#### ❌ Reject

If rejected:

* The requester revises the change plan.
* The requester resubmits the change for approval.

---

## Change Implementation

After CAB approval:

1. Implement the change during the approved maintenance window.
2. Update the Change Request with:

   * Actual start time
   * Completion time
   * Success/fail status

### Close the Change Request

Before closing the CR, document:

* **Post Implementation Review notes**
* Any **deviations from the approved plan**

> 📝 **Audit Requirement:** Ensure the final CR accurately reflects what was actually implemented, including any deviations from the approved implementation plan.

---

## ⭐ Best Practice Tips

* **A poor back-out plan is worse than no change** — test rollback in non-production before CAB submission.
* **Emergency changes require ECAB (Emergency CAB)** — use pre-identified on-call approvers rather than a full committee meeting.
* **Every emergency change must have a post-implementation review within 5 business days.**
* **Track unauthorised changes** (Change Requests opened after implementation) — these are high risk.
* **Integrate Change Management with CMDB** — all changes should update affected CI (Configuration Item) records.
