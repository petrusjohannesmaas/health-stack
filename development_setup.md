| Component   | Description                                                                         |
| ----------- | ----------------------------------------------------------------------------------- |
| **Besu**    | Runs a single-node Ethereum dev network with Clique consensus and preloaded account |
| **IPFS**    | Local IPFS daemon with private swarm key (secured, local peer network)              |
| **Node.js** | Your backend logic for publishing identities, CIDs, and interacting with contracts  |

---

## 📁 Directory Structure

```
health-stack-os/
├── docker-compose.yml
├── swarm.key                  # For private IPFS swarm
├── besu-config/
│   ├── nodekey                # Besu node's private key
│   └── genesis.json           # Clique genesis file with preloaded ETH
└── data/
    ├── besu/                  # Besu's chain data
    └── ipfs/                  # IPFS repo data
```

---

## 🔐 1. Create `swarm.key` (IPFS Private Network)

Create a new swarm key (or use the example below):

```bash
mkdir -p health-stack-os
cd health-stack-os
echo -e "/key/swarm/psk/1.0.0/\n/base16/\n$(openssl rand -hex 32)" > swarm.key
```

---

## 🔑 2. Create Besu Account and Genesis

### 📂 Create a `besu-config/` folder:

```bash
mkdir -p besu-config
```

### 🧬 Generate a node key:

```bash
docker run --rm -v $(pwd)/besu-config:/keys hyperledger/besu:latest \
  besu operator generate-blockchain-config \
  --config-file=/keys/config.json \
  --to=/keys
```

Create `besu-config/config.json` with the following contents:

```json
{
  "genesis": {
    "config": {
      "chainId": 1337,
      "clique": {
        "period": 1,
        "epoch": 30000
      }
    },
    "nonce": "0x0",
    "timestamp": "0x58ee40ba",
    "extraData": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "gasLimit": "0x47b760",
    "difficulty": "0x1",
    "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "coinbase": "0x0000000000000000000000000000000000000000",
    "alloc": {
      "0x0000000000000000000000000000000000000001": {
        "balance": "0xD3C21BCECCEDA1000000"
      }
    }
  },
  "blockchain": {
    "nodes": [
      {
        "privateKeyFile": "nodekey"
      }
    ]
  }
}
```

> 👆 This preloads `0x0000000000000000000000000000000000000001` with **1 million ETH**.

After running the command above, your `besu-config/` will contain:

* `nodekey`
* `genesis.json` (auto-generated)

---

## 🧩 3. `docker-compose.yml`

Save this in `health-stack-os/docker-compose.yml`:

```yaml
version: '3.8'
services:
  besu:
    image: hyperledger/besu:latest
    container_name: besu-node
    ports:
      - "8545:8545"
      - "8546:8546"
      - "30303:30303"
    volumes:
      - ./data/besu:/var/lib/besu
      - ./besu-config:/config
    command: >
      --network=dev
      --data-path=/var/lib/besu
      --genesis-file=/config/genesis.json
      --node-private-key-file=/config/nodekey
      --rpc-http-enabled
      --rpc-http-host=0.0.0.0
      --rpc-http-port=8545
      --rpc-ws-enabled
      --rpc-ws-port=8546
      --host-allowlist=*
      --miner-enabled
      --miner-coinbase=0x0000000000000000000000000000000000000001

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
    command: daemon --migrate=true --enable-pubsub-experiment
```

---

## 🚀 4. Launch the Stack

From inside the `health-stack-os/` folder:

```bash
docker-compose down -v  # ensures fresh start
docker-compose up -d
```

---

## ✅ 5. Verify Besu Account Is Active

Run this to confirm your account is available:

```bash
curl -X POST http://localhost:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'
```

Expected result:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": ["0x0000000000000000000000000000000000000001"]
}
```

You now have a **fully synced dev chain** with a funded account and a **private IPFS daemon** ready to go.

---

## 🧠 Next Steps

* Connect your **Node.js** backend to:

  * Ethereum RPC (`localhost:8545`)
  * IPFS API (`localhost:5001`)
* Use the preloaded address to deploy and interact with smart contracts
* Use IPFS to publish and retrieve CIDs

---

Would you like:

* a sample **Node.js script** to deploy a contract and publish to IPFS?
* or a ready-to-use **frontend template** connected to this backend?

Let me know how you'd like to build from here.


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