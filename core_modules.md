# Core Health Stack OS Modules

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

Next: [2. Patient States and Intake Flow](patient_states_and_intake_flow.md)