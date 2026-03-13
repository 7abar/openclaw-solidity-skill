# Foundry Reference

## forge — Build & Test

```bash
forge build                          # compile
forge test                           # run all tests
forge test -vvv                      # with stack traces
forge test -vvvv                     # with full traces
forge test --match-test test_Name    # single test
forge test --match-contract MyTest   # single file
forge test --fork-url $RPC_URL       # fork mainnet
forge coverage                       # coverage report
forge coverage --report lcov         # lcov for IDE
forge snapshot                       # gas snapshots
forge fmt                            # format code
```

## cast — Interact

```bash
# Read
cast call $ADDR "fn()(type)" --rpc-url $RPC
cast storage $ADDR $SLOT --rpc-url $RPC
cast balance $ADDR --rpc-url $RPC -e          # -e = in ETH
cast code $ADDR --rpc-url $RPC
cast tx $TX_HASH --rpc-url $RPC
cast receipt $TX_HASH --rpc-url $RPC

# Write
cast send $ADDR "fn(args)" arg1 arg2 \
  --rpc-url $RPC --private-key $PK

# Send ETH
cast send $ADDR --value 0.1ether \
  --rpc-url $RPC --private-key $PK

# Decode
cast decode-calldata "fn(address,uint256)" $CALLDATA
cast 4byte $SELECTOR              # lookup function signature
cast abi-decode "fn()(uint256)" $DATA
cast sig "transfer(address,uint256)"   # get selector

# Utils
cast to-wei 1.5                   # ETH to wei
cast from-wei 1500000000000000000 # wei to ETH
cast to-checksum $ADDRESS         # checksum address
cast keccak "hello"               # keccak256 hash
cast block latest --rpc-url $RPC  # latest block
```

## anvil — Local Fork

```bash
# Fork Base mainnet locally
anvil --fork-url https://mainnet.base.org --port 8545

# Fork at specific block
anvil --fork-url https://mainnet.base.org --fork-block-number 12345678

# Impersonate account
cast rpc anvil_impersonateAccount $ADDR

# Set balance
cast rpc anvil_setBalance $ADDR 0xDE0B6B3A7640000  # 1 ETH in hex wei
```

## Test Patterns

```solidity
// Fork + impersonate
function setUp() public {
    vm.createSelectFork("base", 12345678); // specific block
}

// Warp time
vm.warp(block.timestamp + 7 days);

// Mock calls
vm.mockCall(addr, abi.encodeWithSelector(IERC20.balanceOf.selector, user), abi.encode(100e18));

// Expect revert
vm.expectRevert("error message");
vm.expectRevert(MyContract.CustomError.selector);

// Expect emit
vm.expectEmit(true, true, false, true);
emit Transfer(from, to, amount);

// Prank
vm.prank(alice);           // next call only
vm.startPrank(alice);      // all calls until stopPrank
vm.stopPrank();

// Deal ETH
vm.deal(alice, 1 ether);

// Deal ERC-20 (fork only)
deal(address(USDC), alice, 1000e6);

// Labels (for traces)
vm.label(alice, "Alice");
vm.label(address(contract), "MyContract");
```

## Gas Optimization Tips

- Use `uint256` not `uint8` for loop counters (cheaper)
- Pack struct variables by size (32 bytes per slot)
- `immutable` > `constant` for address types
- `calldata` > `memory` for read-only function args
- Avoid storage reads in loops — cache to memory
- Use custom errors instead of `require("string")`
- `unchecked` for known-safe arithmetic (++i)
