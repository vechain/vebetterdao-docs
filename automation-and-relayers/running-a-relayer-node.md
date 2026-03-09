# Running a Relayer Node

There are three ways to run a relayer, from easiest to most advanced:

### Option 1: Run on the Web

The simplest way to get started. Use the [Relayer Dashboard](https://vechain.github.io/b3tr) to register and run a relayer directly from your browser — no installation, no server, no command line.

1. Connect your wallet
2. Register as a relayer
3. Start the relayer from the dashboard UI

Best for: trying it out, small-scale operation, non-technical users.

### Option 2: Run with Docker

Run the official relayer node image locally or on a server. Reliable, always-on, no code to manage.

```bash
docker run -d \
  -e MNEMONIC="your twelve word mnemonic phrase here" \
  -e RELAYER_NETWORK=mainnet \
  --restart unless-stopped \
  vechain/vebetterdao-relayer-node
```

Environment variables:

| Variable              | Required                       | Description                           |
| --------------------- | ------------------------------ | ------------------------------------- |
| `MNEMONIC`            | Yes (or `RELAYER_PRIVATE_KEY`) | Wallet mnemonic                       |
| `RELAYER_PRIVATE_KEY` | Yes (or `MNEMONIC`)            | Wallet private key                    |
| `RELAYER_NETWORK`     | Yes                            | `mainnet` or `testnet-staging`        |
| `RUN_ONCE`            | No                             | Run once then exit (useful for cron)  |
| `DRY_RUN`             | No                             | Simulate without sending transactions |

Best for: reliable 24/7 operation on a VPS or home server.

### Option 3: Fork and Optimize

Fork the [relayer node source code](https://github.com/vechain/vebetterdao-contracts/tree/main/relayer-node) and build your own optimized version.

```bash
git clone https://github.com/vechain/vebetterdao-contracts
cd relayer-node
yarn install
yarn start
```

The default node discovers users, batches votes and claims, and loops every 5 minutes. By forking you can:

* Optimize batching strategies and gas usage
* Implement custom user prioritization
* Add monitoring, alerting, and logging
* Run multiple instances with load balancing
* Integrate with your app's backend

Best for: competitive relayers, apps running relaying as a service, maximizing profitability.

### Gas Costs & Profitability

| Action                       | Gas       | VTHO Cost       | B3TR Equivalent |
| ---------------------------- | --------- | --------------- | --------------- |
| Vote (5-8 apps)              | \~441,000 | \~4.41 VTHO     | \~0.075 B3TR    |
| Claim                        | \~208,000 | \~2.08 VTHO     | \~0.035 B3TR    |
| **Total per user per round** |           | **\~6.49 VTHO** | **\~0.11 B3TR** |

Average user earns \~90-190 B3TR/round. At 10% fee: \~9-19 B3TR per user into the pool. Relayer cost per user: \~0.11 B3TR. **Margin: \~8.9-18.9 B3TR per user.**

***
