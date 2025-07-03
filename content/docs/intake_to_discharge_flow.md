---
draft: true
title: 'Patient States & Intake Flow'
weight: 4
---

### 🔁 State Table


| **State**                 | **Triggered By**                                   | **Next Possible States**                    |
|--------------------------|----------------------------------------------------|---------------------------------------------|
| `Intake`                 | Patient arrives at facility                        | `Registered`, `Referred Elsewhere`          |
| `Registered`             | Admin enters patient info                          | `Triage`, `Declined Admission`              |
| `Triage`                 | Nurse or clinician assesses condition              | `Admitted`, `Referred`                      |
| `Admitted`               | Bed is assigned and care team initiated            | `Under Care`                                |
| `Under Care`             | Routine monitoring and treatment ongoing           | `Escalated`, `Ready for Discharge`          |
| `Escalated`              | Deteriorating condition, transferred to another unit| `Under Care`, `Transferred`                |
| `Ready for Discharge`    | Clinician initiates discharge planning             | `Discharged`, `Delayed Discharge`           |
| `Discharged`             | All clearance complete, patient leaves             | End state                                   |

### Patient Flow Diagram (from Intake to Discharge)

```mermaid
graph TD
    A[Intake] -->|Patient info entered| B[Registered]
    A -->|Patient redirected| Z[Referred Elsewhere]

    B -->|Triage started| C[Triage]
    B -->|Admission declined| Y[Declined Admission]

    C -->|Bed assigned| D[Admitted]
    C -->|Referral required| X[Referred]

    D --> E[Under Care]
    E -->|Condition worsens| F[Escalated]
    E -->|Stable for discharge| G[Ready for Discharge]

    F -->|Stabilized| E
    F -->|Unit transfer| W[Transferred]

    G -->|Patient cleared| H[Discharged]
    G -->|Logistics issue| V[Delayed Discharge]
```