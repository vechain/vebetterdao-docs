# Reward Distribution

**To enhance the transparency** of the VeBetter platform and x2earn apps, following also the ["dApp Tracker" proposal](https://governance.vebetterdao.org/proposals/48098052949649968323435845011935575242371156821795293696975298342916966237360), we have developed a centralized reward distributor contract. This contract must be used by the apps to ensure that every transfer of a B3TR token related to a sustainable action is publicly tracked and accessible to the community.

You need to call this contract to distribute the rewards.

### Sustainable vs Bonus Rewards (V9+)

Starting with `X2EarnRewardsPool` V9 the contract distinguishes two reward flows:

- **Sustainable rewards** — paid for a verifiable sustainable action. They **register a passport action** and contribute to the receiver's [Proof of Participation](../../vepassport/checks/proof-of-participation.md) score. A non-empty proof (`proofTypes` / `proofValues`) is **mandatory**. Use `distributeRewardWithProof`, `distributeRewardWithProofAndMetadata`, or their `*ForRound` counterparts.
- **Bonus / secondary rewards** — endorser payouts, leaderboard prizes, streak bonuses, cashback, referral payouts, etc. They **do not register a passport action** and therefore do not keep a receiver's passport active on their own. Use `distributeNonProofReward(appId, amount, receiver, category, description)` with a `NonProofRewardCategory` (`Endorser`, `Leaderboard`, `Streak`, `Cashback`, `Referral`, `Other`). The contract emits the dedicated `NonProofRewardDistributed` event so indexers can exclude these payouts from personhood signals.

{% hint style="warning" %}
`distributeReward` (no-proof) is **deprecated** in V9. It still works for backward compatibility but registers a passport action without a proof, which is what V9 is trying to remove. Migrate to `distributeRewardWithProof` (sustainable) or `distributeNonProofReward` (bonus).
{% endhint %}

### Round Attribution

By default, when you distribute a reward, the action is recorded in the **current round**. If your app allows users to accumulate actions and claim them later, the action will be attributed to the round when the claim happens — not when the action was performed.

To attribute actions to the correct round, use the `ForRound` variants of the distribution functions. These accept an additional `actionRound` parameter (the round ID when the action was actually performed):

- `distributeRewardForRound`
- `distributeRewardWithProofForRound`
- `distributeRewardWithProofAndMetadataForRound`
- `distributeRewardDeprecatedForRound` (V9+, round-attribution counterpart of `distributeRewardDeprecated`)

The `actionRound` must be greater than 0.

{% hint style="info" %}
`distributeNonProofReward` does **not** have a `ForRound` variant — bonus rewards do not register a passport action, so round attribution is not applicable.
{% endhint %}

{% content-ref url="javascript.md" %}
[javascript.md](javascript.md)
{% endcontent-ref %}

{% content-ref url="solidity.md" %}
[solidity.md](solidity.md)
{% endcontent-ref %}
