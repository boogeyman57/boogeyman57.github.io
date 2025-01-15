---
layout: "post"
title: "Risk-Score-&-Rating-Calculation-Using-CVSS-3.0"
categories: [Cybersecurity, CVSS]
tags: [CVSS, Nessus, Vulnerability-Management, Risk-Scoring]
---

In the last write-up, we explored setting policies and scanning with **Nessus**. This time, let’s delve into calculating the **Risk Score** and **Rating** using the **Common Vulnerability Scoring System (CVSS) 3.0**

# Types of CVSS Scores

CVSS comprises three types of scores:

1. **Base Score**

   This score is determined using **8 parameters** provided by the vendor or a reputable risk-rating agency:
   - Attack Vector
   - Attack Complexity
   - Privileges Required
   - User Interaction
   - Scope
   - Confidentiality Impact
   - Integrity Impact
   - Availability Impact

2. **Temporal Score**

   Time-specific variables affecting the vulnerability are factored here. These include:
   - Exploit Code Maturity
   - Remediation Level
   - Report Confidence

3. **Environmental Score**

   This is influenced by specific factors in the user’s environment:
   - Eight Base Score parameters
   - Confidentiality Requirements
   - Integrity Requirements
   - Availability Requirements

---

# Step-by-Step Risk Scoring Process

To calculate the risk of a vulnerability, follow these steps:

1. **Calculate the Base Score**
2. **Adjust the Temporal Score**
3. **Adjust the Environmental Score**
4. **Assign the Risk Rating**

---

# Example Walkthrough: CVE-2019-0232

Let’s analyze **CVE-2019-0232** and calculate its risk score.

### Scenario Details:

- Exploit code is available but needs substantial modification before execution.
- The vendor has confirmed the vulnerability and released a security update.
- An attacker can only target devices on the same network (firewalls are in place).
- No confidentiality loss, but the affected system contains critical organizational information.
- File modifications have a limited impact, and service disruption causes minimal operational effects.

## Step 1: Base Score Calculation

Search for the CVE in the **National Vulnerability Database (NVD)** to find its Base Score: **8.1 (High)**.

### Base Score Vector:
`CVSS:3.0/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H`

---

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*5fVeUI0DGKMnQVHskO6nhw.png)

We can see the base score is 8.1 High.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*XXPLIYNhHTTS99ywlIrwbQ.png)

## Step 2: Temporal Score Adjustment

Using the CVSS calculator, adjust the following:

- **Exploit Code Maturity (E):** Set to **Proof of Concept (P)** since the exploit requires modification.
- **Remediation Level (RL):** Set to **Official Fix (O)** because the vendor released a security update.
- **Report Confidence (RC):** Set to **Not Defined (X)** as no additional confirmation details are available.

### Temporal Score Vector:
`CVSS:3.0/E:P/RL:O/RC:X`

---

## Step 3: Environmental Score Adjustment

Modify the parameters based on the scenario:

- **Attack Vector (MAV):** Set to **Adjacent Network (A)** due to firewall restrictions.
- **Confidentiality Impact (MC):** Set to **None (N)** since there’s no confidentiality breach.
- **Confidentiality Requirement (CR):** Set to **High (H)** as the system contains critical information.
- **Integrity Requirement (IR):** Set to **Low (L)** since file modifications have a limited impact.
- **Availability Requirement (AR):** Set to **Low (L)** as service disruption is minimal.

### Environmental Score Vector:
`CVSS:3.0/MAV:A/MC:N/CR:H/IR:L/AR:L`

---

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*LBRMzaUfeXOl9Bip_iUy8w.png)

## Step 4: Assign Risk Rating

The final overall score, after all adjustments, is **4.3 (Medium)**.

Refer to the **CVSS Specification Document** for qualitative severity ratings. A score of **4.3** indicates a **Medium Risk**.

---

# Key Takeaways

1. **Understanding CVSS Parameters:** Each parameter significantly influences the risk calculation.
2. **Real-World Context:** Adjustments like Temporal and Environmental Scores provide a realistic assessment of risk.
3. **Tool Proficiency:** Using tools like the **CVSS Calculator** and NVD ensures accurate and efficient risk analysis.

Stay tuned for more cybersecurity insights and hands-on examples!
