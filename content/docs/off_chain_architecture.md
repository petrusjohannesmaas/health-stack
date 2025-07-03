---
draft: true
title: 'Off-chain Architecture'
weight: 5
---

## 🧱 Core Modules

### 1. **Identity & Access Control**
- User account management (Patients, Staff, Admins)
- Role-based (RBAC) and/or attribute-based (ABAC) permissions
- Session handling and audit logs

### 2. **Patient Journey Engine**
- Orchestrates state transitions (`Intake → Discharge`)
- Enforces valid transitions via rules or decision trees
- Emits logs for transitions and alerts

### 3. **Clinical Records Management**
- Captures vitals, assessments, treatment notes, prescriptions
- Structured and unstructured data storage
- Tied to each visit or encounter

### 4. **Admissions & Bed Coordination**
- Tracks available beds and ward assignments
- Links to patient state (`Admitted`, `Transferred`)

### 5. **Referrals & Transfers**
- Logs referrals to/from other facilities
- Handles documentation exchange and transfer summaries

### 6. **Discharge Planning**
- Manages readiness criteria, scheduling, and documentation
- Supports delays or handovers

---

## 🧭 Coordination & Oversight Modules

### 7. **Care Team Management**
- Associates staff members with patients
- Handles care assignments, handovers, and escalations

### 8. **Workflows & Task Tracking**
- Tracks todos for each user per patient state
- Checklists for clinicians and admins (e.g., before discharge)

### 9. **Communication & Alerting**
- Internal notes, patient messages, or escalation pings
- Notifies staff of critical transitions or delays

### 10. **Forms & Document Store**
- Dynamic form builder for triage, referrals, etc.
- Uploads for scans, discharge letters, external reports

---

## 📊 Reporting & Oversight Modules

### 11. **Audit & Compliance Log**
- Immutable event logs tied to roles
- Tracks every update and state transition for review

### 12. **Dashboards & Metrics**
- Patient flow visualizations (admissions, escalations, delays)
- Fund distribution simulation (future DeFi logic)

## 🧩 v2 Expanded list

- **Localization & Multilingual Support** for broader accessibility  
- **Anonymization & Data Export** for compliance or donor insight  
- **Rule Engine** for custom alerts, protocols, or smart contract triggers  
- **Plug-in System** for financial or DAO integrations when you're ready