
### 🧑‍⚕️ Core Clinical & Operational Roles

| **Role**               | **Primary Functions**                                                                 | **System Access Scope**                              |
|------------------------|----------------------------------------------------------------------------------------|------------------------------------------------------|
| **General Nurse**      | Administer meds, monitor vitals, log observations, escalate issues                   | Task board, patient vitals, medication orders        |
| **Clinician / Doctor** | Diagnose, create care plans, order labs/meds, discharge patients                      | Full patient records, order entry, discharge console |
| **Ward Admin**         | Patient registration, bed assignment, file uploads, discharge paperwork               | Admission module, room status, ID documentation      |
| **Pharmacist**         | Review and dispense prescriptions, manage medication inventory                        | Medication orders, pharmacy stock                    |
| **Lab Technician**     | Process lab orders, upload results, flag anomalies                                     | Lab orders, result upload panel                      |
| **Radiology Technician** | Conduct imaging, upload reports, schedule patient scans                            | Imaging scheduler, scan result input                 |
| **Inventory Manager**  | Track consumables, log usage, trigger reorders, handle equipment maintenance          | Inventory dashboard, usage logs                      |
| **Shift Manager / Supervisor** | Manage staff rosters, respond to incidents, assign floating roles             | Staff scheduler, alerts board                        |
| **Cleaner / Maintenance** | Mark rooms ready, respond to sanitation or facility tasks                         | Facility tasks queue                                 |


### 🔐 System & Support Roles

| **Role**               | **Primary Functions**                                                  | **System Access Scope**                    |
|------------------------|------------------------------------------------------------------------|--------------------------------------------|
| **System Administrator** | Configure roles, update templates, manage permissions, view audit logs | Full backend, admin tools, config panels   |
| **IT Support / Technician** | Set up devices, fix system issues, support staff onboarding        | Device registry, support panel             |
| **Finance / Billing Clerk** | Handle patient billing, insurance forms, claims reconciliation     | Billing console, discharge form access     |

### 🤝 Role Interaction Matrix

> ✅ High = frequent collaboration  
> ✅ Med = moderate interaction  
> ✅ Low = occasional interaction  
> ❌ = no direct interaction
This matrix shows **who collaborates with whom** and how frequently, based on typical inpatient workflows:

| **Role ↓ / Interacts With →** | Nurse | Clinician | Ward Admin | Pharmacist | Lab Tech | Radiology | Inventory | Supervisor | Billing |
|-------------------------------|:-----:|:---------:|:----------:|:----------:|:--------:|:---------:|:---------:|:----------:|:-------:|
| **Nurse**                    |  —    |   ✅ High  |    ✅ Med   |    ✅ Med   |   ✅ Low |   ✅ Low  |    ✅ Low |    ✅ Med  |   ❌     |
| **Clinician**                | ✅ High |    —     |    ✅ Med   |    ✅ High  |   ✅ Med |   ✅ Med  |    ❌     |    ✅ Med  |   ✅ Med |
| **Ward Admin**              | ✅ Med |   ✅ Med  |     —      |    ❌      |   ❌     |    ❌     |    ✅ Med |    ✅ Med  |   ✅ High |
| **Pharmacist**              | ✅ Med |   ✅ High |    ❌      |     —      |   ❌     |    ❌     |    ✅ High|    ✅ Low  |   ✅ Med |
| **Lab Tech**                | ✅ Low |   ✅ Med  |    ❌      |    ❌      |     —    |    ❌     |    ❌     |    ❌      |   ❌     |
| **Radiology Tech**          | ✅ Low |   ✅ Med  |    ❌      |    ❌      |   ❌     |     —     |    ❌     |    ❌      |   ❌     |
| **Inventory Manager**       | ✅ Low |    ❌     |    ✅ Med   |    ✅ High  |   ❌     |    ❌     |     —     |    ✅ Med  |   ❌     |
| **Supervisor**              | ✅ Med |   ✅ Med  |    ✅ Med   |    ✅ Low  |   ❌     |    ❌     |    ✅ Med |     —     |   ✅ Med |
| **Billing Clerk**           | ❌     |   ✅ Med  |    ✅ High  |    ✅ Med  |   ❌     |    ❌     |    ❌     |    ✅ Med  |    —     |


## 🔐 Role-to-Module Access Control Matrix

> Access Levels:  
> - **Full** = Create, Read, Update, Delete  
> - **View** = Read-only  
> - **Own** = Can manage tasks or data assigned to them  
> - **Log** = Can record actions (e.g. meds given)  
> - **❌** = No access
This defines **who can access what**, and at what level:

| **Module**               | Nurse | Clinician | Ward Admin | Pharmacist | Lab Tech | Radiology | Inventory | Supervisor | Billing | Sys Admin |
|--------------------------|:-----:|:---------:|:----------:|:----------:|:--------:|:---------:|:---------:|:----------:|:-------:|:---------:|
| Patient Records          | View  | Full      | View       | View       | View     | View      | ❌        | View       | ❌      | Full      |
| Task Management          | Own   | Full      | View       | Own        | Own      | Own       | Own       | Full       | ❌      | Full      |
| Medication Orders        | Log   | Create    | ❌         | Fulfill    | ❌       | ❌        | View      | View       | ❌      | Full      |
| Lab Orders & Results     | View  | Create/View | ❌       | ❌        | Upload   | ❌        | ❌        | View       | ❌      | Full      |
| Imaging Orders           | View  | Create/View | ❌       | ❌        | ❌       | Upload    | ❌        | View       | ❌      | Full      |
| Admission & Discharge    | View  | Approve   | Create/Edit | ❌       | ❌       | ❌        | ❌        | View       | View    | Full      |
| Inventory Dashboard      | View  | ❌        | View       | View       | ❌       | ❌        | Full      | View       | ❌      | Full      |
| Staff Scheduling         | View  | View      | ❌         | ❌        | ❌       | ❌        | ❌        | Full       | ❌      | Full      |
| Billing & Claims         | ❌    | View      | View       | View       | ❌       | ❌        | ❌        | View       | Full    | Full      |
| System Settings          | ❌    | ❌        | ❌         | ❌        | ❌       | ❌        | ❌        | ❌         | ❌      | Full      |
