# Testnet B3TR tokens

There are two ways to obtain B3TR on testnet for development: a public **faucet** (fastest) or **allocation rounds** (closer to the real production flow).

## B3TR Testnet Faucet

The fastest way to get a small amount of B3TR for development and testing.

* **App:** [https://vechain.github.io/b3tr-testnet-faucet/](https://vechain.github.io/b3tr-testnet-faucet/)
* **Source:** [https://github.com/vechain/b3tr-testnet-faucet](https://github.com/vechain/b3tr-testnet-faucet)
* **Faucet contract (testnet):** `0xfca716f9c93575f428fe49402424454077ccbfee`
* **B3TR token (testnet):** `0x026771d1be764467f8bdb78bb230df10c924b00d`

Connect your wallet (VeWorld or WalletConnect), click **Claim B3TR**, and the faucet contract transfers a fixed amount of B3TR to your address. Each address can claim up to a configurable daily limit.

{% hint style="info" %}
The faucet is meant for development. For larger amounts (e.g. funding a reward distributor for an x2Earn app), use allocation rounds — see below.
{% endhint %}

### Programmatic claim

The faucet is a regular contract; you can call it from any script:

```solidity
interface IB3TRFaucet {
  function claimTokens() external;
  function canClaim(address user) external view returns (bool);
  function remainingClaimsForToday(address user) external view returns (uint256);
}
```

```javascript
import { ethers } from "ethers"

const FAUCET = "0xfca716f9c93575f428fe49402424454077ccbfee"
const FAUCET_ABI = ["function claimTokens()"]

const tx = await new ethers.Contract(FAUCET, FAUCET_ABI, signer).claimTokens()
await tx.wait()
```

### Running out / contributing

If the faucet is empty, anyone with B3TR can call `fundFaucet(amount)` (after approving) to top it up. PRs to the repo are welcome — see [vechain/b3tr-testnet-faucet](https://github.com/vechain/b3tr-testnet-faucet).

## Allocation rounds

If you need to **distribute rewards from your x2Earn app**, you should get B3TR from allocation rounds — that's the same flow your app will use in production.

You can **start a new allocation round** (in production those are started automatically each Monday and last a week, in testnet you need to manually trigger them, and they last 10 minutes), wait 10 minutes, then distribute the rewards to your app (done automatically in production, but you need to manually trigger on testnet).

You can access those functionalities from the "[Admin](https://staging.testnet.governance.vebetterdao.org/admin)" tab.

{% embed url="https://streamable.com/2seek2" %}

{% hint style="danger" %}
Remember that to actually distribute rewards you will need to add your wallet as a reward distributor in your app's settings. Read more in the [Rewards Distribution section](reward-distribution/).
{% endhint %}
