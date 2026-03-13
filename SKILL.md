---
name: solidity-dev
description: Solidity and Foundry development assistant for EVM/Base. Use when user asks to: deploy a contract, run forge tests, verify a contract on BaseScan, check wallet/contract balances, read transaction data, decode calldata, check gas usage, interact with deployed contracts via cast, scaffold new Solidity projects, audit Solidity code for security issues, explain what a contract does, generate NatSpec docs, or any task involving Foundry (forge/cast/anvil), BaseScan API, or Base mainnet.
---

# solidity-dev

Solidity + Foundry development skill. Covers the full onchain dev workflow: write → test → deploy → verify → interact.

## Quick Reference

- **Deploy + verify**: See [deployment.md](references/deployment.md)
- **Forge test commands**: See [foundry.md](references/foundry.md)
- **BaseScan API / read onchain data**: See [basescan.md](references/basescan.md)
- **Security checklist**: See [security.md](references/security.md)

## Environment

```bash
export RPC_URL=https://mainnet.base.org      # Base mainnet
export RPC_SEPOLIA=https://sepolia.base.org  # Base Sepolia (testnet)
export PRIVATE_KEY=0x...                      # deployer key
export BASESCAN_API_KEY=...                   # for verification
```

PATH must include Foundry: `export PATH="$HOME/.foundry/bin:$PATH"`

## Common Workflows

### 1. New project
```bash
forge init my-project && cd my-project
forge install OpenZeppelin/openzeppelin-contracts
```

### 2. Test
```bash
forge test -vvv                    # run all tests with traces
forge test --match-test test_Name  # single test
forge coverage                     # coverage report
```

### 3. Deploy + verify (Base mainnet)
Read [deployment.md](references/deployment.md) for full workflow.

### 4. Interact with deployed contract
```bash
cast call $CONTRACT "functionName()(returnType)" --rpc-url $RPC_URL
cast send $CONTRACT "functionName(args)" arg1 arg2 --rpc-url $RPC_URL --private-key $PRIVATE_KEY
```

### 5. Read onchain data
```bash
cast balance $ADDRESS --rpc-url $RPC_URL -e      # ETH balance
cast code $CONTRACT --rpc-url $RPC_URL            # bytecode
cast storage $CONTRACT $SLOT --rpc-url $RPC_URL   # storage slot
```

## Key Addresses (Base Mainnet)

| Name | Address |
|---|---|
| WETH | 0x4200000000000000000000000000000000000006 |
| USDC | 0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913 |
| Uniswap V3 SwapRouter02 | 0x2626664c2603336E57B271c5C0b26F421741e481 |
| Aave V3 Pool | 0xa238dd80c259a72e81d7e4664a9801593f98d1c5 |

## When to Read Reference Files

- Deploying or verifying → [deployment.md](references/deployment.md)
- Writing/running tests → [foundry.md](references/foundry.md)
- Reading BaseScan, tx history, events → [basescan.md](references/basescan.md)
- Security review or audit → [security.md](references/security.md)
