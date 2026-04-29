# For Developers

Technical reference for the Navigator system contracts.

## Architecture

The Navigator system is implemented across several contracts:

| Contract | Role |
|----------|------|
| **NavigatorRegistry** | Central contract — registration, staking, delegation, voting preferences, fees, slashing, lifecycle |
| **XAllocationVoting** | `castNavigatorVote(citizen, roundId)` — allocation votes for delegated citizens |
| **B3TRGovernor** | `castNavigatorVote(proposalId, citizen)` — governance votes for delegated citizens |
| **VoterRewards** | Deducts navigator fee at claim time, deposits to NavigatorRegistry escrow |
| **VOT3** | Transfer lock for delegated amounts, B3TR-to-VOT3 conversion for navigator staking |
| **RelayerRewardsPool** | Per-user skip tracking, governance action registration |

NavigatorRegistry is a UUPS upgradeable contract with logic split into 6 external libraries: NavigatorStakingUtils, NavigatorDelegationUtils, NavigatorVotingUtils, NavigatorFeeUtils, NavigatorSlashingUtils, NavigatorLifecycleUtils.

## Staking and B3TR-to-VOT3 Conversion

When a navigator stakes B3TR, the NavigatorRegistry converts it to VOT3 under the hood via `VOT3.convertToVOT3()`. This makes the staked amount count as voting power through the ERC20Votes checkpoint system.

* NavigatorRegistry self-delegates on VOT3 during initialization (`VOT3.delegate(address(this))`)
* Per-navigator staked amounts are tracked with `Checkpoints.Trace208` for snapshot queries
* On withdrawal or slashing, VOT3 is converted back to B3TR via `VOT3.convertToB3TR()`
* The staked VOT3 only counts for voting power — it cannot support proposals or be powered down

### Voting Power Calculation

`VotesUtils.getVotes()` (allocation) and `GovernorVotesLogic.getVotes()` (governance) both include `NavigatorRegistry.getStakedAmountAtTimepoint(account, timepoint)` for non-delegated users. This returns 0 for non-navigators, so no conditional check is needed.

## Delegation

Citizens delegate a specific VOT3 amount to one navigator at a time. The delegated amount is:

* **Locked** — VOT3 `_update()` reads `NavigatorRegistry.getDelegatedAmount(from)` to enforce transfer lock
* **Checkpointed** — `Checkpoints.Trace208` for snapshot-based voting power
* **Snapshotted at round start** — mid-round changes take effect next round

### Key Functions

| Function | Description |
|----------|-------------|
| `delegate(navigator, amount)` | First-time delegation |
| `increaseDelegation(amount)` | Add more VOT3 to current delegation |
| `reduceDelegation(amount)` | Reduce delegation (takes effect next round) |
| `undelegate()` | Full removal |

## Voting Mechanics

### Allocation Voting

Two separate functions (NOT merged):

* `castVoteOnBehalfOf(voter, roundId)` — existing auto-voting flow (unchanged)
* `castNavigatorVote(citizen, roundId)` — for delegated citizens. Voting power = delegated amount at round snapshot. Navigator's app preferences and percentages are applied.

Navigator setting preferences (`setAllocationPreferences`) also counts as their own allocation vote.

### Governance Voting

`B3TRGovernor.castNavigatorVote(proposalId, citizen)`:

* Citizen must be delegated to a navigator at proposal snapshot
* Navigator must have set a decision (1=Against, 2=For, 3=Abstain)
* Voting power = delegated amount at proposal snapshot
* Intent multiplier applied for rewards

### Skip-or-Vote Logic

Both allocation and governance `castNavigatorVote` functions include built-in skip logic:

1. Navigator dead at snapshot: revert `NotDelegatedToNavigator`
2. Navigator dead now (exited/deactivated after snapshot): skip immediately, reduce expected actions
3. Navigator alive + preferences/decision set: vote normally
4. Navigator alive + no preferences/decision + skip window reached (2 hours before deadline): skip
5. Navigator alive + no preferences/decision + skip window not reached: revert (relayer retries later)

## Rewards and Fees

Fee ordering at claim time:

1. **Navigator fee** = gross reward x feePercentage / 10000 (goes to NavigatorRegistry escrow)
2. **Relayer fee** = calculated on remainder (goes to RelayerRewardsPool)
3. **Citizen** receives the rest

Navigator fees are locked for a configurable number of rounds (default 4) before the navigator can claim them.

## Slashing

### Minor Slashing

Anyone can call `reportRoundInfractions(navigator, roundId, proposalIds)` after a round ends. The contract checks all five infraction types on-chain:

1. Missed allocation vote
2. Late preferences (set after cutoff)
3. Stale preferences (no update for 3+ rounds)
4. Missed report (must submit every N rounds)
5. Missed governance vote

If any infraction is found, one minor slash is applied: **5% of current remaining stake** (compounding). At most one slash per round.

### Major Slashing

Via governance: `deactivateNavigator(navigator, slashPercentage, slashFees)`. Can slash up to 100% of stake plus forfeit all unclaimed locked fees.

## Key View Functions

| Function | Returns |
|----------|---------|
| `getStake(navigator)` | Current staked amount |
| `getStakedAmountAtTimepoint(navigator, timepoint)` | Historical staked amount (for voting power) |
| `getDelegatedAmount(citizen)` | Current delegated VOT3 amount |
| `getDelegatedAmountAtTimepoint(citizen, timepoint)` | Historical delegation (for snapshot voting) |
| `getNavigator(citizen)` | Current navigator address |
| `getNavigatorAtTimepoint(citizen, timepoint)` | Historical navigator (respects dead-navigator invalidation) |
| `isNavigator(account)` | True if registered and active |
| `canAcceptDelegations(navigator)` | True if active, above min stake, not exiting |
| `getAllocationPreferences(navigator, roundId)` | App IDs and percentages for a round |
| `getProposalDecision(navigator, proposalId)` | Governance decision (1=Against, 2=For, 3=Abstain) |

## Relayer Integration

At round start, `XAllocationVoting.startNewRound` computes expected actions:

* `allocationUsers = autoVotingUsers + totalDelegatedCitizens`
* `governanceUsers = totalDelegatedCitizens` (citizens only — relayers don't cast governance votes for auto-voters)

Per-user skip tracking in RelayerRewardsPool prevents deadlocks: when all vote actions for a user are skipped, the claim action is auto-reduced too.
