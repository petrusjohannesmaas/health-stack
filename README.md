# 🏥 **Health-Stack**  
An **open-source hospital management system**, designed for **flexibility, security, and efficiency**.

---

## 🚀 **Core Features**  
(Modules planned for future integration)  
✅ **Patient Management** – Registration, medical history, appointments, billing  
✅ **Doctor & Staff Management** – Scheduling, permissions, payroll  
✅ **Medical Records** – Secure storage, compliance with health regulations  
✅ **Billing & Insurance** – Automated invoicing, insurance claim processing  
✅ **Inventory Management** – Medical supplies & pharmaceuticals tracking  
✅ **Laboratory & Test Results** – Integration with diagnostic tools  
✅ **Communication Module** – Secure messaging between doctors, patients, and staff  

---

## 🛠 **Getting Started with Patient Management**  

### 1️⃣ **Clone & Deploy PocketBase**  
```bash
git clone https://github.com/petrusjohannesmaas/health-stack.git
cd health-stack
docker-compose up -d
```
Then, open **`http://localhost:8090/_/`** to access the **PocketBase admin panel**.

---

## 📦 **Dockerized Setup**  

### **Docker Compose Configuration (`docker-compose.yml`)**  
```yaml
version: '3'
services:
  pocketbase:
    image: pocketbase/pocketbase:latest
    container_name: healthstack_pocketbase
    ports:
      - "8090:8090"
    volumes:
      - ./pocketbase_data:/pb_data  # Persist healthcare records
    restart: unless-stopped
```
- **PocketBase runs inside a container** with all patient records stored securely.  
- **Data persists** even when restarting the container.  

---

## 📜 **PocketBase Schema for Patient Management**  
This schema ensures **structured patient records** with potential future integration.  

### **Patients Table (`patients.json`)**
```json
{
    "name": "patients",
    "schema": [
        { "name": "full_name", "type": "text", "required": true },
        { "name": "date_of_birth", "type": "date", "required": true },
        { "name": "gender", "type": "select", "options": ["Male", "Female", "Other"], "required": true },
        { "name": "contact_info", "type": "json", "required": true },
        { "name": "medical_history", "type": "json", "required": false },
        { "name": "current_conditions", "type": "json", "required": false },
        { "name": "insurance_provider", "type": "text", "required": false },
        { "name": "billing_status", "type": "select", "options": ["Pending", "Paid", "Overdue"], "required": false }
    ]
}
```
- **JSON-based schema** for easy PocketBase import.  
- **Medical history & conditions stored in structured JSON** for flexibility.  

---

## 🔗 **Potential Future Integrations**  
🔹 **Appointments Module** (Linked via `patient_id`)  
🔹 **Billing & Insurance Module** (Linked via `insurance_provider`)  
🔹 **Doctor Assignments Module** (Mapping to `doctor_id`)  

### **Example Integration: Doctor Assignments (`doctor_assignments.json`)**  
```json
{
    "name": "doctor_assignments",
    "schema": [
        { "name": "patient_id", "type": "relation", "collection": "patients", "required": true },
        { "name": "doctor_id", "type": "relation", "collection": "doctors", "required": true },
        { "name": "visit_date", "type": "date", "required": true },
        { "name": "notes", "type": "text", "required": false }
    ]
}
```

---

## 📊 **How It Works**  
✅ **Admin users manage patient records** via PocketBase UI  
✅ **Patients can be assigned to doctors, tracked across appointments**  
✅ **Billing and medical history are structured for future interoperability**  

---

## 🎯 **Next Steps**  
🔹 Expand **appointments & scheduling module**  
🔹 Integrate **billing & insurance processing**  
🔹 Improve **multi-language & localization support**  

---

## 📜 **License**  
GNU General Public License v3.0  
See [LICENSE](./LICENSE)  

---
