---
draft: true
title: 'Core Modules'
weight: 2
---

#### 1. **Patient Intake & Registration**
- Patient demographics
- Unique ID/wristband generation
- Intake queue + referral tagging

#### 2. **Triage & Vitals Monitoring**
- Vital sign capture (manual/device)
- Triage category assignment
- Escalation flagging (e.g. EWS)

#### 3. **Bed & Room Management**
- Real-time bed occupancy dashboard
- Ward assignments
- Room status flags: dirty, available, reserved

#### 4. **Clinical Documentation & Rounds**
- Encounter notes
- Diagnosis logging
- SOAP/MEWS/WHO-based formats (configurable)
- Patient timeline view

#### 5. **Task Management Engine**
- Role-specific task dashboards
- Task assignment, reminders, escalation logic
- Task status transitions (pending, in-progress, completed)

#### 6. **Medication & Pharmacy**
- Prescription orders
- Dispensing interface (pharmacist)
- Medication stock management
- Administration log (nurse input)

#### 7. **Lab Orders & Results**
- Request forms (blood, urine, etc.)
- Sample ID tracking
- Result upload with reference ranges

#### 8. **Imaging / Radiology**
- Imaging request manager
- Scan scheduling
- Upload and attach scan reports

#### 9. **Discharge Management**
- Discharge summary builder
- Clearance checklist (pharmacy, billing, transport)
- Transport scheduler / referral linkage

#### 10. **Inventory & Consumables**
- Stock entry, deduction, audit logs
- Expiry tracking
- Low-stock alerting and reorder queue

#### 11. **User & Role Management**
- Staff onboarding
- Role assignment and permissions
- Shift scheduler (basic version)

#### 12. **Billing & Insurance**
- Tariff assignment
- Invoice generator
- Insurance claim tagging (if applicable)

---

### 📡 Cross-Cutting Functional Layers

These support all modules above:

#### A. **Access Control & Audit Logging**
- RBAC + contextual permissions
- Access logs per user/session

#### B. **Notification & Alert System**
- Broadcast emergency codes (Code Blue, etc.)
- Task alerts, vitals triggers, low stock warnings

#### C. **Feedback & Analytics Layer**
- Built-in module feedback widgets
- Daily ops dashboard (task delays, vitals alerts, turnover rates)

#### (Post-MVP) D. Expansion Modules

- **Referral Engine** (handoffs to external facilities)
- **Telehealth Gateway** (for remote follow-ups)
- **Diet & Nutrition Module**
- **Patient Education Content System**
- **Mobile-first Companion App for Patients or Staff**

---

Let's **map out all the `.js` (Node.js backend) and `.sol` (Solidity smart contract)** files you'll need **by module**, and **how they interact** (or remain isolated if needed). We’ll also include support layers.

---

## 🔧 File Structure Overview (Top Level)

```bash
/backend
  /controllers
    intake.js
    vitals.js
    ...
  /utils
    ipfs.js
    crypto.js
  /routes
    intakeRoutes.js
    vitalsRoutes.js
    ...
  /contracts
    PatientRegistry.sol
    VitalsLog.sol
    ...
  abi/
    PatientRegistry.json
    VitalsLog.json
.env
server.js

/smart-contracts
  PatientRegistry.sol
  VitalsLog.sol
  BedManager.sol
  ...
```

---

## ✅ 1. Patient Intake & Registration

**.js:**

* `controllers/intake.js`: Handles patient intake form, IPFS upload, smart contract call
* `routes/intakeRoutes.js`: HTTP routes for new patient registration
* `utils/ipfs.js`: Wrapper to add/get files from IPFS

**.sol:**

* `PatientRegistry.sol`: Stores `patientId → IPFS CID`

  * Emits `PatientRegistered(patientId, cid)`
  * Contains `getPatient(patientId)` view

---

## ✅ 2. Triage & Vitals Monitoring

**.js:**

* `controllers/vitals.js`: Records vital signs, triage level, escalation status
* Reads from monitoring devices or form
* `routes/vitalsRoutes.js`

**.sol:**

* `VitalsLog.sol`: Maps `patientId → array of {CID, EWS score, escalated flag}`

---

## ✅ 3. Bed & Room Management

**.js:**

* `controllers/bedManager.js`: Assign beds, update status, show occupancy
* `routes/bedRoutes.js`

**.sol:**

* `BedManager.sol`: Tracks `bedId → status`, `patientId → bedId`
* Emits status updates (dirty, available, reserved)

---

## ✅ 4. Clinical Documentation & Rounds

**.js:**

* `controllers/clinicalNotes.js`: Record SOAP notes, visit data
* `routes/notesRoutes.js`

**.sol:**

* `ClinicalNotes.sol`: Tracks `patientId → array of documentation CIDs`

---

## ✅ 5. Task Management Engine

**.js:**

* `controllers/taskManager.js`: Assign tasks, update status, send reminders
* `routes/taskRoutes.js`

**.sol:**

* `TaskEngine.sol`: Tracks task ownership, lifecycle, escalations

---

## ✅ 6. Medication & Pharmacy

**.js:**

* `controllers/pharmacy.js`: Prescriptions, stock tracking, nurse log
* `routes/pharmacyRoutes.js`

**.sol:**

* `MedicationLog.sol`: Logs prescription CIDs, confirms dispense/administer

---

## ✅ 7. Lab Orders & Results

**.js:**

* `controllers/labOrders.js`: Create lab orders, attach results
* `routes/labRoutes.js`

**.sol:**

* `LabOrders.sol`: Tracks sample IDs, order → result CID mapping

---

## ✅ 8. Imaging / Radiology

**.js:**

* `controllers/imaging.js`: Schedule scans, upload reports
* `routes/imagingRoutes.js`

**.sol:**

* `ImagingLog.sol`: Logs request CIDs + scan result CIDs by patientId

---

## ✅ 9. Discharge Management

**.js:**

* `controllers/discharge.js`: Build summary, checklist, transport link
* `routes/dischargeRoutes.js`

**.sol:**

* `DischargeManager.sol`: Logs discharge CID, checklist flags, transport slot

---

## ✅ 10. Inventory & Consumables

**.js:**

* `controllers/inventory.js`: Add stock, deduct, alert low stock
* `routes/inventoryRoutes.js`

**.sol:**

* `Inventory.sol`: Track `itemId → quantity, expiry, log of movements`

---

## ✅ 11. User & Role Management

**.js:**

* `controllers/users.js`: Onboard staff, assign roles, schedule shifts
* `routes/userRoutes.js`

**.sol:**

* `UserRoles.sol`: RBAC (Role-Based Access Control), maps address → role

---

## ✅ 12. Billing & Insurance

**.js:**

* `controllers/billing.js`: Generate invoice, tag claims
* `routes/billingRoutes.js`

**.sol:**

* `Billing.sol`: Store claim records (CID), associate to patientId and insurer

---

## 📡 Cross-Cutting Functional Layers

### 🛡️ A. Access Control & Audit Logging

**.js:**

* `middleware/auth.js`: JWT-based auth for staff
* `middleware/acl.js`: Checks if user has required role
* `controllers/audit.js`: Log every critical access/action
* `utils/logger.js`

**.sol:**

* `AuditLog.sol`: Append-only logs for actions (`eventId, user, patientId, timestamp`)

---

### 🚨 B. Notification & Alert System

**.js:**

* `controllers/alerts.js`: Fire alerts (task, EWS, inventory, emergency)
* WebSocket or push-based delivery

**.sol:**

* Can use events in multiple contracts (e.g., `VitalsLog.sol`) to trigger alert listeners

---

### 📊 C. Feedback & Analytics Layer

**.js:**

* `controllers/feedback.js`: Collect module-specific feedback
* `controllers/analytics.js`: Process delays, vitals spikes, etc.

**.sol (optional)**:

* `Feedback.sol`: Stores hashed feedback and module ID

---

## 💡 Summary: Module → File Mapping

| Module    | `.js` files            | `.sol` contracts                |
| --------- | ---------------------- | ------------------------------- |
| Intake    | `intake.js`, `ipfs.js` | `PatientRegistry.sol`           |
| Vitals    | `vitals.js`            | `VitalsLog.sol`                 |
| Beds      | `bedManager.js`        | `BedManager.sol`                |
| Notes     | `clinicalNotes.js`     | `ClinicalNotes.sol`             |
| Tasks     | `taskManager.js`       | `TaskEngine.sol`                |
| Pharmacy  | `pharmacy.js`          | `MedicationLog.sol`             |
| Lab       | `labOrders.js`         | `LabOrders.sol`                 |
| Imaging   | `imaging.js`           | `ImagingLog.sol`                |
| Discharge | `discharge.js`         | `DischargeManager.sol`          |
| Inventory | `inventory.js`         | `Inventory.sol`                 |
| Users     | `users.js`             | `UserRoles.sol`                 |
| Billing   | `billing.js`           | `Billing.sol`                   |
| Auth/ACL  | `auth.js`, `acl.js`    | `UserRoles.sol`, `AuditLog.sol` |
| Alerts    | `alerts.js`            | Triggered via `event` logs      |
| Analytics | `analytics.js`         | Optional logging only           |

---

## ⚙️ Coordination Between Parts

* **IPFS**: All `.js` controllers can share a common `ipfs.js` utility
* **Besu**: Every `.js` controller maps to a contract ABI/instance
* **Frontend**: Sends REST/GraphQL calls → backend → IPFS + Besu

---

Would you like a **visual map (PDF or Draw\.io)** of this architecture, or a **starter folder structure template** to scaffold this system?

You’re designing something **very real and powerful** — just say the word and I’ll help scaffold it.


Next: [3. Patient States and Intake Flow](patient_states_and_intake_flow.md)
