# Deployment Reference

## Deploy + Verify (Base Mainnet)

### Script-based deploy (recommended)

```bash
forge script script/Deploy.s.sol:DeployScript \
  --rpc-url https://mainnet.base.org \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --verify \
  --etherscan-api-key $BASESCAN_API_KEY \
  -vvvv
```

### Raw deploy (simple contracts)

```bash
forge create src/MyContract.sol:MyContract \
  --rpc-url https://mainnet.base.org \
  --private-key $PRIVATE_KEY \
  --verify \
  --etherscan-api-key $BASESCAN_API_KEY \
  --constructor-args arg1 arg2
```

### Verify separately (if verification failed)

```bash
forge verify-contract $CONTRACT_ADDRESS \
  src/MyContract.sol:MyContract \
  --chain base \
  --etherscan-api-key $BASESCAN_API_KEY \
  --constructor-args $(cast abi-encode "constructor(address,uint256)" $ARG1 $ARG2)
```

### foundry.toml for Base

```toml
[profile.default]
src = "src"
out = "out"
libs = ["lib"]
solc = "0.8.20"

[rpc_endpoints]
base = "https://mainnet.base.org"
base_sepolia = "https://sepolia.base.org"

[etherscan]
base = { key = "${BASESCAN_API_KEY}", url = "https://api.basescan.org/api" }
base_sepolia = { key = "${BASESCAN_API_KEY}", url = "https://api-sepolia.basescan.org/api" }
```

### Deploy script template

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {Script, console} from "forge-std/Script.sol";
import {MyContract} from "../src/MyContract.sol";

contract DeployScript is Script {
    function run() external {
        vm.startBroadcast();
        MyContract c = new MyContract();
        console.log("Deployed:", address(c));
        vm.stopBroadcast();
    }
}
```

## Base Sepolia (Testnet)

```bash
# Get testnet ETH: https://www.coinbase.com/faucets/base-ethereum-goerli-faucet
forge script script/Deploy.s.sol \
  --rpc-url https://sepolia.base.org \
  --private-key $PRIVATE_KEY \
  --broadcast
```

## Post-Deploy Checklist

- [ ] Contract verified on BaseScan
- [ ] README updated with contract address
- [ ] Owner / admin role transferred if needed
- [ ] Initial state verified via cast call
- [ ] ABI exported: `cat out/MyContract.sol/MyContract.json | jq .abi`
