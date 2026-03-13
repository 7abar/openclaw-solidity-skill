# openclaw-solidity-skill

An [OpenClaw](https://openclaw.ai) agent skill for Solidity developers.

Gives your AI agent expert knowledge of the full Solidity development workflow on Base: write, test, deploy, verify, and interact — all via chat.

## What It Does

Once installed, your OpenClaw agent can:

- Deploy and verify contracts on Base mainnet or Sepolia
- Run `forge test`, interpret failures, suggest fixes
- Read BaseScan — tx history, events, ABI, source code
- Interact with deployed contracts via `cast`
- Review Solidity code for security vulnerabilities
- Generate NatSpec docs and explain contracts in plain English
- Scaffold new Foundry projects

## Install

```bash
# Clone into your OpenClaw skills directory
git clone https://github.com/7abar/openclaw-solidity-skill \
  ~/.openclaw/workspace/skills/solidity-dev
```

That's it. OpenClaw picks up skills automatically from the skills directory.

## Skill Structure

```
solidity-dev/
├── SKILL.md                  ← trigger description + quick reference
└── references/
    ├── deployment.md         ← forge script, verify, foundry.toml config
    ├── foundry.md            ← forge/cast/anvil commands + test patterns
    ├── basescan.md           ← API queries, event topics, rate limits
    └── security.md           ← CEI pattern, checklist, vulnerability guide
```

Progressive disclosure: only relevant reference files are loaded into context when needed.

## Usage

Just ask naturally:

> "Deploy my contract to Base mainnet"
> "Run the tests and explain any failures"
> "Check the last 10 transactions for 0x1234..."
> "Audit this contract for reentrancy"
> "Generate NatSpec for all my functions"

## Requirements

- [OpenClaw](https://openclaw.ai) installed
- [Foundry](https://getfoundry.sh) installed
- BaseScan API key (free at [basescan.org/myapikey](https://basescan.org/myapikey))

## Environment Variables

```bash
export RPC_URL=https://mainnet.base.org
export PRIVATE_KEY=0x...
export BASESCAN_API_KEY=...
```

## Built For

Base mainnet + Foundry. Works with any EVM chain — just change `RPC_URL`.

## License

MIT
