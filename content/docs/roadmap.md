---
draft: true
title: 'Roadmap'
weight: 7
---

### 🪴 **Phase 0 — Strategic Setup (Week 0–2)**

- ✅ Define MVP scope (Patient Flow + Role-Based UI + Minimal Blockchain)
- 🧱 Outline core data entities, states, and user roles
- 🗺 Finalize tech stack for Phase 1 (e.g. Pocketbase, custom UI, Hardhat)
- 📄 Begin project documentation and architecture outline

---

### 🔧 **Phase 1 — Core Patient Flow MVP (Month 1–2)**

Focus: Build and test the essential hospital logic—**entirely off-chain**.

#### Modules:
- **User Roles & Access Control** (Patients, Nurses, Clinicians, Admins)
- **Patient Registry & Encounters**
- **State Management Engine** (`Intake` → `Discharge`)
- **Triage + Admission + Discharge Interfaces**
- **Minimal Dashboards** per role (Pocketbase or custom frontend)
- **Audit Logging & Transition Trails**

🎯 _Goal:_ A working local prototype with the complete state journey and permission logic.

---

### 🧪 **Phase 2 — Local Smart Contract Testing (Month 3–4)**

Focus: Prove the feasibility of incentive-linked disbursement.

#### Tasks:
- Implement **a single smart contract vault** (Hardhat or Foundry)
- Create mock flow for verified milestones triggering disbursement
- Simulate off-chain trigger from patient audit log
- Connect local system with wallet-based testnet interactions

🎯 _Goal:_ Demonstrate end-to-end trigger from care milestone → on-chain disbursement

---

### 🔗 **Phase 3 — Blockchain Integration & Transparency Layer (Month 5–6)**

Focus: Move from simulation to production-ready bridge for fund flows

#### Tasks:
- Deploy minimal contract to testnet (e.g. Sepolia, Besu dev net)
- Wire off-chain logic to emit hashed proofs (optional)
- Add public disbursement ledger viewer
- Build a basic admin DAO or multisig wallet UI (manual to start)

🎯 _Goal:_ Real-time verifiable funding traceability connected to off-chain workflow

---

### 🛠️ **Phase 4 — Workflow Expansion (Month 7–10)**

Focus: Add tooling for oversight, communication, and accountability

#### Modules:
- Task queues and escalation handling
- Delay tracking and discharge coordination
- Staff assignment engine and shift mapping
- Document uploads and external referrals module
- Stakeholder dashboards for funders

🎯 _Goal:_ Fully simulate day-to-day clinic dynamics with oversight dashboards

---

### 🌱 **Phase 5 — Clinical Simulation + Demo Site (Month 11–12)**

Focus: Wrap core MVP into an explorable, demoable system

#### Milestones:
- End-to-end patient case simulation
- Sample grant and milestone progression
- Role-based demo accounts with switchable views
- Seed content for documentation

🎯 _Goal:_ Pitch-ready MVP with working flow, real code, and live walkthrough

---

### 🌍 **Phase 6 — Pilot Deployment & Feedback Loops (Month 13–18)**

Focus: Controlled real-world testing in a single site or sandbox environment

#### Milestones:
- Choose deployment site or partner organization
- Establish data privacy & low-connectivity support
- Collect usage and performance data
- Iterate on pain points, expand FAQ/docs

🎯 _Goal:_ A compelling, usable prototype that proves Health Stack OS can run in the real world

---
