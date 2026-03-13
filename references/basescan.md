# BaseScan API Reference

Base URL: `https://api.basescan.org/api`
API Key env: `BASESCAN_API_KEY`

## Common Queries

### Account transactions
```bash
curl "https://api.basescan.org/api?module=account&action=txlist\
&address=$ADDR&startblock=0&endblock=99999999\
&page=1&offset=25&sort=desc&apikey=$BASESCAN_API_KEY"
```

### ERC-20 transfers
```bash
curl "https://api.basescan.org/api?module=account&action=tokentx\
&address=$ADDR&page=1&offset=25&sort=desc&apikey=$BASESCAN_API_KEY"
```

### Contract ABI
```bash
curl "https://api.basescan.org/api?module=contract&action=getabi\
&address=$CONTRACT&apikey=$BASESCAN_API_KEY"
```

### Contract source code
```bash
curl "https://api.basescan.org/api?module=contract&action=getsourcecode\
&address=$CONTRACT&apikey=$BASESCAN_API_KEY"
```

### ETH balance
```bash
curl "https://api.basescan.org/api?module=account&action=balance\
&address=$ADDR&tag=latest&apikey=$BASESCAN_API_KEY"
```

### Event logs
```bash
# Filter by topic0 (event signature hash)
curl "https://api.basescan.org/api?module=logs&action=getLogs\
&fromBlock=0&toBlock=latest\
&address=$CONTRACT\
&topic0=$(cast keccak "Transfer(address,address,uint256)")\
&page=1&offset=100\
&apikey=$BASESCAN_API_KEY"
```

### Gas oracle
```bash
curl "https://api.basescan.org/api?module=gastracker&action=gasoracle\
&apikey=$BASESCAN_API_KEY"
```

## TypeScript Fetch Pattern

```typescript
const BASE_API = "https://api.basescan.org/api";
const KEY = process.env.BASESCAN_API_KEY;

async function getTxList(address: string, limit = 25) {
  const url = `${BASE_API}?module=account&action=txlist`
    + `&address=${address}&page=1&offset=${limit}&sort=desc&apikey=${KEY}`;
  const res = await fetch(url);
  const data = await res.json();
  if (data.status !== "1") throw new Error(data.message);
  return data.result;
}
```

## Known Event Signatures (topic0)

| Event | topic0 |
|---|---|
| ERC-20 Transfer | `0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef` |
| ERC-20 Approval | `0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925` |
| Uniswap V3 Swap | `0xc42079f94a6350d7e6235f29174924f928cc2ac818eb64fed8004e115fbcca67` |
| ERC-721 Transfer | `0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef` |

## Rate Limits

- Free tier: 5 req/sec, 100k req/day
- Use `&apikey=` always (even free tier has higher limits with key)
- For bulk data: add delays between requests

## Links

- BaseScan: https://basescan.org
- API docs: https://docs.basescan.org
- Register API key: https://basescan.org/myapikey
