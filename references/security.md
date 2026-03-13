# Solidity Security Reference

## Critical Patterns to Check

### 1. Reentrancy (most common)
```solidity
// VULNERABLE
function withdraw() external {
    uint amount = balances[msg.sender];
    (bool ok,) = msg.sender.call{value: amount}(""); // external call first
    balances[msg.sender] = 0; // state update after — WRONG
}

// SAFE (CEI pattern: Checks → Effects → Interactions)
function withdraw() external {
    uint amount = balances[msg.sender];
    balances[msg.sender] = 0; // state update first
    (bool ok,) = msg.sender.call{value: amount}("");
    require(ok);
}
```

### 2. Access Control
```solidity
// Check: every state-changing function should have appropriate access
// Common patterns:
modifier onlyOwner() { require(msg.sender == owner, "not owner"); _; }
modifier onlyAuthorized() { require(authorized[msg.sender], "not authorized"); _; }

// ⚠️ Watch for: functions that should be restricted but aren't
// ⚠️ Watch for: admin functions callable during wrong state
```

### 3. Integer Issues
```solidity
// Solidity 0.8+ has built-in overflow checks — safe by default
// ⚠️ Watch for: unchecked { } blocks hiding overflow
// ⚠️ Watch for: casting to smaller types (uint256 → uint128)
// ⚠️ Watch for: division rounding (always rounds down in Solidity)
```

### 4. Front-Running
```solidity
// ⚠️ High risk: DEX trades, auctions, randomness
// Mitigations: commit-reveal scheme, slippage protection, deadline params
```

### 5. Timestamp Manipulation
```solidity
// ⚠️ block.timestamp can be manipulated by validators (±15 seconds)
// ⚠️ Don't use for precise time-based logic (randomness, precise deadlines)
// OK for: "has 7 days passed?" type checks
```

### 6. Oracle Manipulation
```solidity
// ⚠️ Spot prices from AMMs are easily manipulated via flash loans
// Use: Chainlink price feeds, TWAP (time-weighted average price)
// Never: use AMM spot price as sole oracle in same-block logic
```

## Security Checklist

**Access Control**
- [ ] All admin functions have proper modifiers
- [ ] Owner/admin transfer is two-step (nominate + accept)
- [ ] No functions accidentally left unprotected

**Reentrancy**
- [ ] All external calls follow CEI pattern
- [ ] Re-entrancy guard on functions that change state + make calls

**Math**
- [ ] No unchecked blocks hiding overflow
- [ ] Division results handled correctly (no precision loss)
- [ ] No casting to smaller types without bounds check

**External Calls**
- [ ] Return values of `.call()` checked
- [ ] No delegatecall to user-controlled addresses
- [ ] No `.transfer()` or `.send()` (gas issues) — use `.call{value:}`

**Initialization**
- [ ] Constructor sets all critical state
- [ ] Upgradeable contracts: initializer called exactly once

**Events**
- [ ] All state-changing functions emit events
- [ ] Indexed parameters for filterable fields

## Common Vulnerability Score Guide

| Severity | Example |
|---|---|
| Critical | Reentrancy draining funds, unauthorized mint |
| High | Access control bypass, significant fund loss possible |
| Medium | Front-running with economic impact, wrong state |
| Low | Missing event, suboptimal pattern |
| Info | Gas optimization, style issues |

## Tools

```bash
# Slither static analysis
pip install slither-analyzer
slither src/

# Mythril symbolic execution
pip install mythril
myth analyze src/MyContract.sol

# Foundry fuzz testing
forge test --fuzz-runs 10000
```
