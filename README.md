# Drosera Trap - Predictable RNG Detection

This repository contains a simple custom Drosera trap implementation using Foundry. The trap queries a smart contract's `isActive()` function and encodes the response to help identify predictable random number generation behavior or similar logic.



---

## ✨ Features

- Reads an external contract's `isActive()` function.
- Encodes and sends result along with a Discord
- Supports response filtering via `shouldRespond()`.

---

## ⚙️ Environment Setup

Install Foundry and dependencies:

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup

Install Node.js dependencies (optional, uses Bun):
curl -fsSL https://bun.sh/install | bash
bun install
Install Drosera CLI:
curl -L https://app.drosera.io/install | bash
droseraup
🚀 Quick Start
Update your drosera.toml:
response_contract = "0x342198f808536687fbc5cd824004445ef09e36ba"
response_function = "isActive()"
Then deploy your trap:
# Compile
forge build

# Deploy
DROSERA_PRIVATE_KEY=0x... drosera apply
After successful deployment, the CLI will append the address field automatically to your drosera.toml.

🧠 Trap Logic
collect() encodes:

isActive() result from your contract.

Your Discord username: 

shouldRespond() filters traps where:

isActive() is true.

Discord username is non-empty.

📬 Contract Address
Trap Response Contract:
0x342198f808536687fbc5cd824004445ef09e36ba
