# Governance

{% hint style="info" %}
Following community proposal, since the [13th of August 2025](https://governance.vebetterdao.org/proposals/16456558618029822768724567372248444488437431846368752919370485354508180864756), VOT3 deposits on proposals also **count as voting power for XApp allocation votes**. The deposit threshold is capped at 5M VOT3. To submit a proposal, the proposer must [create a Discourse](https://vechain.discourse.group/latest) thread and be a [Moon (or higher) GM NFT](../../vechain-node-and-staking-guides/legacy-vechain-node-guides-pre-stargate/vechain-nodes-x-gm-nfts-maximising-benefits-within-vebetter.md) holder.
{% endhint %}

{% hint style="warning" %}
This page describes the V11 Governor, which introduces the **Community Execution Framework**: every Standard proposal can now carry an on-chain B3TR implementation cost, a payout address, a developer description, an implementation-discussion link, and a list of contributors. Pre-V11 proposals (no `maxBudget`) keep working with no payout flow.
{% endhint %}

Anyone holding VOT3 is a member of VeBetter and can take part in voting. VOT3 is VeBetter's governance token, obtained by swapping B3TR at a 1:1 ratio. All members can vote on proposals, participate in governance discussions, and access the Treasury, with a minimum requirement of holding **1 VOT3**.

## Governance Stabilization Period and Initialization

At launch, VeBetter adhered to an initial set of governance rules until it had run for one year without any technical interruptions. The Stabilization Period was critical for ensuring the long-term efficacy and viability of VeBetter. To prevent exploits during the Stabilization Period, the VeChain Foundation maintained the ability to pause and restart B3TR token minting, only in the case of an exploit or other major issue. Once the requirements were met, community stakeholders can maintain or deprecate this functionality through governance.

## The V11 lifecycle at a glance

```
Pending → Active → Succeeded → Queued → Executed → InDevelopment → Completed → (Paid)
                  ↘ Defeated / DepositNotMet / Canceled
```

* **Pending → Active**: VOT3 deposit threshold must be reached during the deposit period.
* **Active → Succeeded**: quorum reached and FOR > AGAINST at the end of the voting round.
* **Succeeded → Queued → Executed**: actionable proposals pass through the Timelock before any on-chain calls are made. Non-executable proposals skip Queued/Executed and remain in Succeeded.
* **Executed (or Succeeded) → InDevelopment**: the proposer (or an admin) calls `markAsInDevelopment` to register a **payout address**, a **description**, an **implementation discussion** link, and a list of **contributors**. This is when the V11 community-execution flow takes ownership of the developer payout.
* **InDevelopment → Completed**: an admin (role `PROPOSAL_STATE_MANAGER_ROLE`) flips the proposal to Completed via `markAsCompleted` once delivery is verified.
* **Completed → Paid**: anyone can call `claimPayout` to pull the full implementation cost from the Treasury to the payee in a single transfer.

## Proposal Submission

Proposals are the backbone of the DAO. VeBetter maintains a Discourse forum for proposal discussion and a dashboard for voting results and history. To reach on-chain voting, a proposal goes through forum discussion, a community temperature check (the on-chain deposit), and finally an on-chain vote.

### Requirements

<figure><img src="../../.gitbook/assets/Screenshot 2025-08-18 at 11.41.23 AM.png" alt="" width="375"><figcaption></figcaption></figure>

* **Hold a GM Moon NFT (or higher)** — upgrade your **Earth GM NFT** to **Moon**, or purchase one. If you don't hold an Earth GM NFT, **participate in at least one vote** to earn eligibility for the free Earth mint.
* **Create a Discourse thread** at least **3 days** before submitting the proposal, so the community can review it.

### Community support (deposit period)

Every new proposal goes through a deposit period. A deposit threshold of VOT3 — calculated as a percentage of total B3TR supply, capped at 5M VOT3 — must be met for the proposal to become active. This deposit acts as a temperature check: if the community believes the proposal is worth considering, members can contribute VOT3. If the threshold is reached before the end of the waiting period, the proposal becomes Active; otherwise, it is automatically marked DepositNotMet.

Deposits can be withdrawn once the proposal is cancelled or voting starts. While VOT3 is locked as a deposit it cannot be transferred or used to support other proposals, but **it is still counted in the allocation voting snapshot** and grants extra voting power in allocation rounds (e.g. tokens locked supporting a proposal that starts in Round #4 still count in Rounds #3 and #4).

### Creating a proposal (V11)

When drafting a proposal, the proposer provides:

* **Description** — what the proposal aims to achieve (off-chain markdown).
* **Targets / values / calldatas** — for actionable proposals only.
* **Start round** — the allocation round in which voting becomes active.
* **Deposit amount** — initial VOT3 contributed by the proposer.
* **Implementation cost (`maxBudget`)** — the on-chain B3TR budget that will be paid out to the developer payout address after delivery. Set to **0** for proposals that don't need a developer payout (pure governance directions, parameter changes, etc.).

The on-chain call is:

```solidity
function propose(
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    string  memory description,
    uint256 startRoundId,
    uint256 depositAmount,
    uint256 maxBudget        // V11: 0 disables the community-execution flow
) external returns (uint256 proposalId);
```

Setting `maxBudget > 0` emits `ProposalBudgetSet(proposalId, maxBudget)` and locks the implementation cost for the lifetime of the proposal — it can never be increased later.

### Proposal types

* **Actionable** — the proposal carries one or more on-chain calls that execute through the Timelock once Succeeded.
* **Non-actionable** — no on-chain calls; the proposal expresses a direction. After Succeeded the proposer can still record an implementation cost / payee via the community-execution flow.
* **Grants** — submitted via `proposeGrant` and follow the milestone-based payout flow (see [How to apply for grants](how-to-apply-for-grants.md)). Grants do **not** use the V11 community-execution flow.

## Proposal Process

{% stepper %}
{% step %}
### Deposit Period

A temperature check. The community **must fund the proposal with VOT3** until it reaches the deposit threshold. Each contribution also boosts the contributor's allocation voting power. If the threshold isn't reached by the end of the waiting period, the proposal is automatically marked DepositNotMet. The proposer or an admin can cancel an active deposit period.
{% endstep %}

{% step %}
### Voting Period

Once the proposal is active, a snapshot of total VOT3 supply and each holder's balance is taken to calculate voting power. Holders cast **FOR / AGAINST / ABSTAIN**. Each proposal is active for one round. 100% of voting power is applied to the option you select — votes cannot be split.
{% endstep %}

{% step %}
### Outcome

When voting ends, if the **30% quorum** is reached and FOR > AGAINST, the proposal is **Succeeded**. Actionable proposals are queued in the Timelock and then **Executed**. Non-actionable proposals stay in Succeeded.
{% endstep %}

{% step %}
### In Development (V11)

After Executed (or Succeeded for non-actionable proposals), the proposer — or an admin holding `PROPOSAL_STATE_MANAGER_ROLE` — calls `markAsInDevelopment` to register:

* the **payee** (the single address that will receive the full implementation cost),
* a short **description** of the implementation,
* an **implementation discussion** URL,
* a list of **contributors** (GitHub / X handles or URLs, capped at `maxContributorsPerProposal`, currently 20).

`payee` must be non-zero when `maxBudget > 0`, and must be zero when `maxBudget == 0` (you can't pay out a proposal that never set a budget, and you can't have a budget without a destination). Any of these fields can be replaced later via `updateCommunityExecution`, as long as the payout has not yet been claimed.
{% endstep %}

{% step %}
### Completed

Once delivery is verified, an admin (`PROPOSAL_STATE_MANAGER_ROLE`) calls `markAsCompleted` to flip the proposal into the **Completed** state. From this point the proposal is eligible for payout.
{% endstep %}

{% step %}
### Payout

**Anyone** can call `claimPayout(proposalId)` once the proposal is Completed. The Governor pulls the full `maxBudget` from the Treasury and transfers it in a single B3TR payment to the registered payee. The call is idempotent — a second `claimPayout` reverts with `PayoutAlreadyClaimed`. After the payout, all V11 fields become immutable.

The payee is responsible for distributing funds to the contributors **off-chain**. The on-chain `contributors[]` list is for attribution only and does not control how the budget is split.
{% endstep %}
{% endstepper %}

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Proposal States</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Proposal Life Cycle.png" alt=""><figcaption><p>Lifecycle of the proposal</p></figcaption></figure>

### Quorum

Quorum is **30% of the total supply of VOT3**. All vote types (FOR, AGAINST, ABSTAIN) count toward quorum. Voting rewards are distributed regardless of the vote cast.

### Vote outcome

If quorum is not reached the proposal is **Defeated**. If quorum is reached, FOR is compared against AGAINST: more FOR than AGAINST → **Succeeded**, otherwise **Defeated**. Voting rewards are distributed regardless of the outcome.

### Execution and Timelock

VeBetter's Governor contract can execute actions on any contract on VeChainThor, provided that contract allows the action. A Timelock (OpenZeppelin's `TimelockController` combined with `GovernorTimelockControl`) delays execution so users can exit the system before a contentious decision lands. To execute, a proposal must first be queued in the Timelock and wait the configured minimum delay.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption><p>Governance setup</p></figcaption></figure>

### Vote Delegation

The Governor only counts **delegated** voting power. Auto-delegation is enabled on VOT3 transfers and on the B3TR → VOT3 swap, so EOAs receive voting power automatically. **Smart contracts** are excluded from auto-delegation — a contract that wants to vote must call `delegate(itself)` once.

If you want someone else to vote on your behalf, delegate to their address; you keep your tokens but they cast the vote.

## Quadratic Voting

Quadratic Voting (QV) re-calibrates voting power so large holders don't dominate. With QV enabled, voting power is the square root of the holder's VOT3 balance:

$$
\text{votingPower} = \sqrt{v}, \text{ where } v \text{ is the number of VOT3 tokens held}
$$

Votes are still cast as FOR / AGAINST / ABSTAIN, but the effective weight follows the recalibrated value.

### Linear Voting

QV is enabled by default. Admins (or governance) can disable QV at any time, switching to linear voting where voting power equals VOT3 balance. If QV is toggled during an ongoing round, the change only takes effect from the **next** round.

## Community Execution Framework (V11)

The V11 Community Execution Framework is the on-chain layer that lets the DAO **fund and verify the delivery** of a proposal end-to-end. It replaces the off-chain "we'll figure it out later" pattern with three on-chain commitments:

1. **An implementation cost** locked into the proposal at creation (`maxBudget`).
2. **A single payout address** registered when the team commits to delivery (`markAsInDevelopment`), editable until the payout is claimed.
3. **A public claim** anyone can trigger once an admin marks the work Completed (`claimPayout`).

### Funds flow

* The implementation cost is **not** locked in the Governor at proposal creation — it's a commitment, not an escrow. The Treasury still holds B3TR until `claimPayout` is called.
* On `claimPayout`, the Governor calls `Treasury.transferB3TR(payee, maxBudget)` to forward the full budget in one transaction. The Governor must hold `Treasury.GOVERNANCE_ROLE` for this to succeed — granted as part of the V11 upgrade.
* If the Treasury is short of B3TR at claim time, `claimPayout` reverts and can be retried later.

### Editing community-execution data

`updateCommunityExecution` lets the proposer (or an admin) replace the payee, description, implementation discussion, and contributors **while the proposal is InDevelopment or Completed and the payout has not yet been claimed**. Once `claimPayout` succeeds, all V11 fields are frozen forever.

### Authorization summary

| Action | Caller |
|---|---|
| `propose` (with `maxBudget`) | Anyone meeting the standard requirements |
| `markAsInDevelopment` | Proposer **or** `PROPOSAL_STATE_MANAGER_ROLE` |
| `updateCommunityExecution` | Proposer **or** `PROPOSAL_STATE_MANAGER_ROLE` |
| `markAsCompleted` | `PROPOSAL_STATE_MANAGER_ROLE` only |
| `resetDevelopmentState` | `PROPOSAL_STATE_MANAGER_ROLE` only |
| `claimPayout` | Anyone (idempotent) |

### Events

* `ProposalBudgetSet(proposalId, maxBudget)` — emitted from `propose` when `maxBudget > 0`.
* `ProposalInDevelopment(proposalId)` — emitted from `markAsInDevelopment`.
* `ProposalInDevelopmentDetails(proposalId, payee, description, implementationDiscussion)` — emitted from `markAsInDevelopment` and `updateCommunityExecution`.
* `ProposalContributorsSet(proposalId, contributors)` — emitted alongside the details event.
* `ProposalCompleted(proposalId)` — emitted from `markAsCompleted`.
* `ProposalPayoutClaimed(proposalId, payee, amount)` — emitted from `claimPayout`.
* `ProposalDevelopmentStateReset(proposalId)` — emitted from `resetDevelopmentState`.

### Errors

* `InvalidPayeeAddress` — payee is the zero address while `maxBudget > 0`.
* `MissingProposalBudget` — caller passed a payee while `maxBudget == 0`.
* `TooManyContributors(provided, max)` — contributors array exceeds `maxContributorsPerProposal`.
* `PayeesAlreadyFinalized` — `markAsInDevelopment` was already called for this proposal.
* `PayoutAlreadyClaimed` — `claimPayout` was already called, or `updateCommunityExecution` is attempted after claim.
* `NotReadyToClaim` — `claimPayout` was called but the proposal has no registered payee or no budget.
* `UnauthorizedCommunityExecution(caller, proposalId)` — caller is neither the proposer nor an admin.

## Upgradeable parameters

The following parameters can be updated by governance (or `DEFAULT_ADMIN_ROLE` on the Governor where applicable):

* **Quorum**: currently 30% of total VOT3 supply.
* **Deposit Threshold**: percentage of total B3TR supply (in VOT3) required to activate a proposal — currently **2%**, capped at **5M VOT3**.
* **Voting Threshold**: minimum amount of VOT3 needed to cast a vote (currently 1).
* **Minimum Voting Delay**: minimum time a proposal must remain Pending before voting can start (currently 3 days).
* **Timelock Minimum Delay**: time between queuing and execution for actionable proposals.
* **Voting Period**: must be ≤ the Emissions Cycle. Extending it requires extending the Emissions Cycle first.
* **Function Restrictions**: when enabled, only whitelisted `(target, selector)` pairs can be called from a proposal.
* **Quadratic Voting**: can be enabled or disabled per round.
* **`maxContributorsPerProposal`** (V11): cap on the contributors array per proposal — set at the V11 upgrade (currently 20). There is no runtime setter; changing it requires a Governor upgrade.

## Where to find the contracts

| Contract | Purpose |
|---|---|
| `B3TRGovernor` | Main entrypoint — `propose`, `markAsInDevelopment`, `updateCommunityExecution`, `markAsCompleted`, `claimPayout`. Current version: **11**. |
| `GovernorCommunityExecutionLogic` | Library holding the V11 community-execution logic, called via delegatecall from `B3TRGovernor`. |
| `TimelockController` | OZ Timelock used for execution delay. |
| `Treasury` | Holds B3TR; transfers to payee on `claimPayout`. |

The full ABI is published in [`@vechain/vebetterdao-contracts`](https://www.npmjs.com/package/@vechain/vebetterdao-contracts).
