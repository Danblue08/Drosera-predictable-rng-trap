# INSTALL DROSERA NETWORK WITH CUSTOM TRAP

**Contract** : `0x342198f808536687fbc5cd824004445ef09e36ba`

**Function** : Detect Predictable RNG via `isActive()`

---

## 🎯 Quick Explanation: Predictable RNG Trap

Trap bots attracted to contracts that include random mint/lottery features, without realizing the "randomness" can be predicted and manipulated.



---

## System Requirements

- **Spec**: 4 GB RAM, 2 Core, 20 GB SSD

## Tested On

- **OS**: Ubuntu 22

## Mining ETH Hoodi

- Faucet: [https://hoodi-faucet.pk910.de/](https://hoodi-faucet.pk910.de/)

---

## Setup

```bash
screen -R drosera
```

```bash
sudo apt-get update && sudo apt-get upgrade -y && sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y && sudo apt update -y && sudo apt upgrade -y && for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

```bash
sudo apt-get update && sudo apt-get install ca-certificates curl gnupg && sudo install -m 0755 -d /etc/apt/keyrings && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg && sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

```bash
echo \
  "deb [arch=\"$(dpkg --print-architecture)\" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  \"$(. /etc/os-release && echo \"$VERSION_CODENAME\")\" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```bash
sudo apt update -y && sudo apt upgrade -y && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && sudo docker run hello-world
```

```bash
curl -L https://app.drosera.io/install | bash && source /root/.bashrc && droseraup
```

```bash
curl -L https://foundry.paradigm.xyz | bash && source /root/.bashrc && foundryup
```

```bash
curl -fsSL https://bun.sh/install | bash && mkdir my-drosera-trap && cd my-drosera-trap && forge init -t drosera-network/trap-foundry-template
```

```bash
apt install snapd
```

```bash
snap install bun-js
```

```bash
curl -fsSL https://bun.sh/install | bash && bun install && forge build
```

```bash
DROSERA_PRIVATE_KEY=YOURPRIVATEKEY drosera apply
```

```bash
drosera dryrun
```

---

## Configure drosera.toml

```toml
path = "out/Trap.sol/Trap.json"
response_contract = "0x342198f808536687fbc5cd824004445ef09e36ba"
response_function = "isActive()"
```

## Create `Trap.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {ITrap} from "drosera-contracts/interfaces/ITrap.sol";

interface IEthereumHoodie {
    function isActive() external view returns (bool);
}

contract Trap is ITrap {
    address public constant RESPONSE_CONTRACT = 0x342198f808536687fbc5cd824004445ef09e36ba;
    string constant discordName = "usernamediscord"; // Set your Discord name here

    function collect() external view returns (bytes memory) {
        bool active = IEthereumHoodie(RESPONSE_CONTRACT).isActive();
        return abi.encode(active, discordName);
    }

    function shouldRespond(bytes[] calldata data) external pure returns (bool, bytes memory) {
        (bool active, string memory name) = abi.decode(data[0], (bool, string));
        if (!active || bytes(name).length == 0) {
            return (false, bytes(""));
        }
        return (true, abi.encode(name));
    }
}
```

---

## Run Drosera Operator

```bash
curl -LO https://github.com/drosera-network/releases/releases/download/v1.18.0/drosera-operator-v1.18.0-x86_64-unknown-linux-gnu.tar.gz
```

```bash
tar -xvf drosera-operator-v1.18.0-x86_64-unknown-linux-gnu.tar.gz
```

```bash
sudo cp drosera-operator /usr/bin
```

```bash
source /root/.bashrc && droseraup
```

```bash
forge build
```

```bash
DROSERA_PRIVATE_KEY=YOURPRIVATEKEY drosera apply
```

```bash
drosera dryrun
```

```bash
drosera-operator register --eth-rpc-url https://rpc.hoodi.ethpandaops.io --eth-private-key PRIVATE_KEY --drosera-address 0x91cB447BaFc6e0EA0F4Fe056F5a9b1F14bb06e5D
```

```bash
drosera-operator optin --eth-rpc-url https://rpc.hoodi.ethpandaops.io --eth-private-key PRIVATE_KEY --trap-config-address TRAPS_ADDRESS
```

```bash
drosera bloomboost --private-key PRIVATE_KEY --trap-address TRAPS_ADDRESS --eth-amount 3
```

---

## Optional: Docker Setup

```bash
git clone https://github.com/0xmoei/Drosera-Network
cd Drosera-Network
```

```bash
docker pull ghcr.io/drosera-network/drosera-operator:latest
```

### Create `docker-compose.yaml`

```yaml
version: '3'
services:
  drosera:
    image: ghcr.io/drosera-network/drosera-operator:latest
    container_name: drosera-node
    ports:
      - "31313:31313"
      - "31314:31314"
    volumes:
      - drosera_data:/data
    command: node --db-file-path /data/drosera.db --network-p2p-port 31313 --server-port 31314 --eth-rpc-url https://rpc.hoodi.ethpandaops.io --eth-backup-rpc-url https://ethereum-hoodi-rpc.publicnode.com/ --drosera-address 0x91cB447BaFc6e0EA0F4Fe056F5a9b1F14bb06e5D --eth-private-key ${ETH_PRIVATE_KEY} --listen-address 0.0.0.0 --network-external-p2p-address ${VPS_IP} --disable-dnr-confirmation true
    restart: always

volumes:
  drosera_data:
```

```bash
docker compose up -d && docker compose logs -f
```

