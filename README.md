# 🏥 **Health-Stack DApp**  
An **open-source hospital management system**, now built on an **Ethereum-compatible ledger**, combining modular governance with cryptographic data integrity.

---

## 🚀 **Core Modules** (Planned Smart Contracts)  
✅ `PatientRegistry.sol` – Register, update, and verify patient data  
✅ `DoctorRegistry.sol` – Manage doctor identities, roles, and affiliations  
✅ `MedicalRecords.sol` – Immutable access to health records (IPFS or zkStorage integration suggested)  
✅ `Billing.sol` – Smart invoice generation and insurance claim logic  
✅ `PharmaInventory.sol` – Track medicine batches on-chain  
✅ `LabResults.sol` – On-chain test metadata linked to off-chain storage  
✅ `MessageHub.sol` – Encrypted peer-to-peer comms between patients and doctors (opt-in)

---

## 🛠️ **Getting Started Locally**

### 1️⃣ Initialize Smart Contract Project

```bash
mkdir health-stack-dapp && cd health-stack-dapp
npm init -y
npm install --save-dev hardhat
npx hardhat
```

Select "Create a basic sample project"

```bash
npm install --save-dev @nomicfoundation/hardhat-toolbox @openzeppelin/contracts
```

---

### 2️⃣ Besu Integration via `hardhat.config.js`

```js
require("@nomicfoundation/hardhat-toolbox");

module.exports = {
  defaultNetwork: "besu",
  networks: {
    besu: {
      url: "http://localhost:8545", // Besu RPC endpoint
      accounts: ["0xPRIVATE_KEY"], // Replace with unlocked account
    },
  },
  solidity: "0.8.20",
};
```

---

## ✍️ **Sample Contract: PatientRegistry.sol**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PatientRegistry {
    struct Patient {
        string fullName;
        string dob;
        string gender;
        string contactInfo;
    }

    mapping(address => Patient) public patients;

    event Registered(address indexed patientAddr, string fullName);

    function registerPatient(
        string memory _fullName,
        string memory _dob,
        string memory _gender,
        string memory _contactInfo
    ) public {
        patients[msg.sender] = Patient(_fullName, _dob, _gender, _contactInfo);
        emit Registered(msg.sender, _fullName);
    }

    function getPatient(address _addr) public view returns (Patient memory) {
        return patients[_addr];
    }
}
```

---

## 🧪 **Deploy with Hardhat**

`scripts/deploy.js`:

```js
async function main() {
  const PatientRegistry = await ethers.getContractFactory("PatientRegistry");
  const contract = await PatientRegistry.deploy();
  await contract.deployed();
  console.log(`Deployed to: ${contract.address}`);
}
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

Deploy:

```bash
npx hardhat run scripts/deploy.js --network besu
```

---

## 🧬 **Evolving the Stack**

| Module            | Smart Contract        | Off-chain Partner                    |
|------------------|-----------------------|--------------------------------------|
| Patients          | PatientRegistry.sol   | React+Web3 UI + Hardhat console      |
| Medical Records   | MedicalRecords.sol    | IPFS / zkSnarks / Lighthouse         |
| Billing           | Billing.sol           | Insurance API or stablecoin logic    |
| Inventory         | PharmaInventory.sol   | Manufacturer signatures, QR tracking |
| Messaging         | MessageHub.sol        | Libp2p, Waku, or Whisper              |

---

## 🔐 **Optional Enhancements**
- Integrate Besu **private transaction manager (Orion)** for confidential patient record logs  
- Use zk-proofs for **anonymized medical statistics**  
- Layer on **Role-Based Access Control** with OpenZeppelin extensions  
- Move schema definitions into Solidity storage structs with off-chain indexing  

---
