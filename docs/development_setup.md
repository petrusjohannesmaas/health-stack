# Development Setup


| Component   | Description |
|------------|-------------|
| **Besu**   | Runs single-node dev network (Clique consensus, instant finality) |
| **IPFS**   | Local daemon with HTTP API (`localhost:5001`) |
| **Node.js** | Your logic layer – identity creation, CID publishing, smart contract calls, QR output |

---

## 📁 Directory Structure

```
health-stack-os/
├── docker-compose.yml
├── swarm.key
└── data/
    ├── besu/
    └── ipfs/
```

---

## 🔐 `swarm.key` (Private IPFS Network Key)

Create this file at `health-stack-os/swarm.key`:

```
/key/swarm/psk/1.0.0/
/base16/
f1f2f3f4f5f6f7f8f9fafbfcfdfeff00112233445566778899aabbccddeeff0011
```

> Or generate a new one:
```bash
echo -e "/key/swarm/psk/1.0.0/\n/base16/\n$(openssl rand -hex 32)" > swarm.key
```

---

## 🧩 `docker-compose.yml`

```yaml
---
services:
  besu:
    image: hyperledger/besu:latest
    container_name: besu-node
    ports:
      - "8545:8545"   # HTTP JSON-RPC
      - "8546:8546"   # WebSocket
      - "30303:30303" # P2P
    volumes:
      - ./data/besu:/var/lib/besu
    command: >
      --network=dev
      --data-path=/var/lib/besu
      --rpc-http-enabled
      --rpc-http-host=0.0.0.0
      --rpc-http-port=8545
      --rpc-ws-enabled
      --rpc-ws-port=8546
      --host-allowlist=*

  ipfs:
    image: ipfs/kubo:latest
    container_name: ipfs-node
    ports:
      - "4001:4001"
      - "4001:4001/udp"
      - "5001:5001"
      - "8080:8080"
    volumes:
      - ./data/ipfs:/data/ipfs
      - ./swarm.key:/data/ipfs/swarm.key:ro
    command: daemon --migrate=true
```

---

## 🚀 Launch the Stack

From inside `health-stack-os/`:

```bash
docker-compose up -d
```

---

## ✅ Verify Everything Is Working

- **Besu RPC**:
  ```bash
  curl -X POST http://localhost:8545 \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
  ```

- **IPFS API**:
  ```bash
  curl http://localhost:5001/api/v0/version
  ```

- **IPFS Web UI**:  
  Open [http://localhost:5001/webui](http://localhost:5001/webui)

---

## 🔐 Make IPFS Fully Private (Optional but Recommended)

Run these once after startup:

```bash
docker exec ipfs-node ipfs bootstrap rm --all
docker exec ipfs-node ipfs config --json Discovery.MDNS.Enabled false
docker exec ipfs-node ipfs config Routing.Type dht
docker exec ipfs-node ipfs config --json AutoTLS.Enabled false
docker restart ipfs-node
```

This ensures:
- No public bootstrap peers
- No local peer discovery
- No AutoTLS or public routing

Absolutely, Petrus—here’s the polished section you can drop straight into your Health Stack OS documentation. I’ve made sure it’s clean, reusable, and doesn’t rely on hardcoded CIDs:

---

## 🧪 Test File Creation and Cleanup on IPFS

### 📥 Create and Add a File

To verify your IPFS node is functioning, create a small JSON file and add it:

```bash
docker exec -i ipfs-node sh -c "echo '{\"status\": \"It works\\!\"}' > /data/ipfs/test.json && ipfs add /data/ipfs/test.json"
```

This will return output like:

```
added <CID> test.json
```

Copy the returned CID for use in subsequent commands.

---

### 🧹 How to Fully Remove the File

If the file was automatically pinned, IPFS will prevent deletion unless it's unpinned first.

#### 1. 🔓 Unpin the File

```bash
docker exec ipfs-node ipfs pin rm <CID>
```

#### 2. 🧨 Remove the Block from Local Storage

```bash
docker exec ipfs-node ipfs block rm <CID>
```

#### 3. 🗑️ Trigger Garbage Collection

```bash
docker exec ipfs-node ipfs repo gc
```

This permanently removes the file from your node’s datastore.

---

### 📌 Optional: List All Recursively Pinned CIDs

Before unpinning, you can inspect which CIDs are pinned:

```bash
docker exec ipfs-node ipfs pin ls --type=recursive
```

---

Great choice, Petrus—let’s keep it lean and native. You can deploy your test smart contract directly to Besu using just:

- the Solidity compiler (`solc`)
- `ethers.js` in a Node.js script
- or `curl` for raw JSON-RPC if you're feeling bold

Here’s the streamlined route using **just Node.js + ethers**, without Hardhat or Truffle.

---

## 🛠️ Test Contract

### 1. Save the Contract Code

Create `Greeter.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Greeter {
    string public greeting;

    constructor(string memory _greeting) {
        greeting = _greeting;
    }

    function setGreeting(string memory _newGreeting) public {
        greeting = _newGreeting;
    }

    function getGreeting() public view returns (string memory) {
        return greeting;
    }
}
```

---

### 2. Compile Using `solc`

Install solc locally or globally:

```bash
npm install -g solc
```

Then compile:

```bash
solc --optimize --combined-json abi,bin Greeter.sol > Greeter.json
```

> This gives you ABI + bytecode to use in your deploy script.

---

### 3. Deploy Script Using `ethers.js`

```bash
npm install ethers
```

Then create `deploy.js`:

```js
const fs = require('fs');
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('http://localhost:8545');
const privateKey = '0xYOUR_DEV_PRIVATE_KEY_HERE'; // Use the default dev account key
const wallet = new ethers.Wallet(privateKey, provider);

const compiled = JSON.parse(fs.readFileSync('Greeter.json'));
const abi = compiled.contracts['Greeter.sol:Greeter'].abi;
const bytecode = '0x' + compiled.contracts['Greeter.sol:Greeter'].bin;

(async () => {
  const factory = new ethers.ContractFactory(abi, bytecode, wallet);
  const contract = await factory.deploy("Hello, Besu!");
  await contract.waitForDeployment();

  console.log("✅ Deployed at:", contract.target);
  console.log("🔁 Current greeting:", await contract.getGreeting());
})();
```

Run it:

```bash
node deploy.js
```

---

### 🔓 Need a Dev Key?

Besu’s `--network=dev` comes with unlocked accounts. You can extract one using:

```bash
curl -X POST http://localhost:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'
```

You can then fund it or use `--rpc-http-authentication-enabled=false` to simplify access.

---


### 🧪 What You Can Do Next

- Deploy your `IdentityRegistry.sol` contract to Besu
- Upload identity JSONs to IPFS
- Emit `IdentityCreated` events with CIDs
- Generate QR codes from CIDs

---

Would you like me to scaffold a Node.js script that ties this all together—uploading to IPFS, registering on Besu, and generating a QR code? That would give you a full Phase 1 identity loop in one go.