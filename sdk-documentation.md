## SDK Documentation

### Ridgeline Perps SDK (TypeScript)

The Ridgeline SDK wraps the REST API into a typed client for JavaScript/TypeScript projects.

**Installation**
```bash
npm install @ridgelineperps/sdk
```

**Initialization**
```typescript
import { RidgelineClient } from '@ridgelineperps/sdk';

const client = new RidgelineClient({
  apiKey: process.env.RIDGELINE_API_KEY,
});
```

**Checking account balance**
```typescript
const balance = await client.account.getBalance();
console.log(balance.availableCollateral);
```

**Placing an order**
```typescript
const order = await client.orders.create({
  market: 'BTC-PERP',
  side: 'long',
  size: 250,
  leverage: 4,
  orderType: 'market',
});

console.log(`Order filled at ${order.entryPrice}`);
```

**Listening for position updates (WebSocket)**
The SDK includes a WebSocket subscription helper for real-time position and price updates:

```typescript
client.subscribe('positions', (update) => {
  console.log(`Position ${update.positionId} PnL: ${update.unrealizedPnl}`);
});
```

**Closing a position**
```typescript
await client.positions.close('pos_44a1', { closePercentage: 100 });
```

**Handling errors**
The SDK throws typed errors you can catch specifically:

```typescript
import { InsufficientCollateralError } from '@ridgelineperps/sdk';

try {
  await client.orders.create({ market: 'ETH-PERP', side: 'long', size: 10000, leverage: 20, orderType: 'market' });
} catch (err) {
  if (err instanceof InsufficientCollateralError) {
    console.log('Not enough collateral for this position size.');
  }
}
```

**Configuration options**

| Option | Type | Default | Description |
|---|---|---|---|
| `apiKey` | string | — | Required. Your Ridgeline API key |
| `baseUrl` | string | `api.ridgelineperps.xyz/v1` | Override for staging/testing environments |
| `timeout` | number | 10000 | Request timeout in milliseconds |

---
