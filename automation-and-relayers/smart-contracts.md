# Smart Contracts

### Contract Addresses

| Contract           | Address                                      |
| ------------------ | -------------------------------------------- |
| RelayerRewardsPool | `0x34b56f892c9e977b9ba2e43ba64c27d368ab3c86` |
| XAllocationVoting  | `0x89A00Bb0947a30FF95BEeF77a66AEdE3842Fe5B7` |
| VoterRewards       | `0x838A33AF756a6366f93e201423E1425f67eC0Fa7` |

### RelayerRewardsPool

Manages relayer registration, action tracking, and reward distribution.

#### Registration

Anyone can register as a relayer by calling `registerRelayer()`. No admin approval needed.

```solidity
registerRelayer()           // Self-register as a relayer (open to anyone)
unregisterRelayer(address)  // Admin only (POOL_ADMIN_ROLE)
isRegisteredRelayer(address) -> bool
getRegisteredRelayers() -> address[]
```

#### Configuration

```
relayerFeePercentage = 10        // 10% of user rewards
feeCap = 100 ether               // Max 100 B3TR per user per round
voteWeight = 3                   // Points per vote action
claimWeight = 1                  // Points per claim action
earlyAccessBlocks = 432,000      // ~5 days on VeChain
```

#### Reward Distribution

```
relayerShare = (relayerWeightedActions / completedWeightedActions) * totalRewards
```

Rewards are claimable when:

* The round has ended
* All work is done (`completedWeightedActions >= totalWeightedActions`)

#### Key Functions

```solidity
// Registration
registerRelayer()
isRegisteredRelayer(address) -> bool
getRegisteredRelayers() -> address[]

// Reward claiming
claimRewards(uint256 roundId)
isRewardClaimable(uint256 roundId) -> bool
getTotalRewards(uint256 roundId) -> uint256
getRelayerWeightedActions(uint256 roundId, address relayer) -> uint256

// Internal (called by other contracts)
registerRelayerAction(address relayer, address voter, uint256 roundId, ActionType action)
deposit(uint256 amount, uint256 roundId)
setTotalActionsForRound(uint256 roundId, uint256 userCount)
reduceExpectedActionsForRound(uint256 roundId, uint256 userCount)
```

### XAllocationVoting (Auto-Voting Functions)

Auto-voting was added in v8 via the `AutoVotingLogicUpgradeable` module.

#### User Functions

```solidity
toggleAutoVoting(address user)                    // Enable/disable auto-voting
setUserVotingPreferences(bytes32[] memory appIds) // Set apps (1-15, no duplicates)
getUserVotingPreferences(address) -> bytes32[]    // View preferences
isUserAutoVotingEnabled(address) -> bool          // Current status
```

#### Relayer Functions

```solidity
castVoteOnBehalfOf(address voter, uint256 roundId)
```

This function:

1. Validates early access (registered relayer during the 5-day window)
2. Gets user preferences, filters eligible apps
3. Splits voting power equally across eligible apps
4. Casts the vote
5. Registers a VOTE action on RelayerRewardsPool (3 weight points)

#### Snapshot Functions

```solidity
isUserAutoVotingEnabledForRound(address, uint256) -> bool
getTotalAutoVotingUsersAtRoundStart() -> uint256
getTotalAutoVotingUsersAtTimepoint(uint48) -> uint256
```

Auto-voting status is checkpointed: changes take effect from the next round, not the current one.

### VoterRewards (Fee Integration)

Fee deduction happens in `VoterRewards.claimReward()`:

1. Check user had auto-voting enabled at round start (checkpointed)
2. Calculate raw rewards (voting + GM reward)
3. If auto-voting: `fee = min(totalReward * 10/100, 100 B3TR)`
4. Fee approved + deposited to `RelayerRewardsPool.deposit(fee, cycle)`
5. Register CLAIM action for `msg.sender` (1 weight point)
6. Net reward transferred to voter wallet

`msg.sender` calling `claimReward()` IS the relayer credited for the CLAIM action.

### Early Access Windows

During the early access period, only registered relayers can act. Users cannot self-vote or self-claim.

| Window       | Duration | Start          |
| ------------ | -------- | -------------- |
| Vote window  | \~5 days | Round snapshot |
| Claim window | \~5 days | Round deadline |

After the early access period, anyone can act — this is the safety net ensuring users always receive their rewards.

### Round Lifecycle

```
Round N: User enables auto-voting + sets preferences (checkpointed)
Round N+1:
  1. startNewRound() — snapshot locks auto-voting status
  2. setTotalActionsForRound(roundId, userCount)
  3. Relayers call castVoteOnBehalfOf() for each user
     - Ineligible users: reduceExpectedActionsForRound()
     - Each successful vote: +3 weighted points to relayer
  4. Round ends (deadline block)
  5. Relayers call VoterRewards.claimReward() for each user
     - Fee deducted and deposited to pool
     - Each successful claim: +1 weighted point to relayer
  6. All actions complete → pool unlocked
  7. Relayers call RelayerRewardsPool.claimRewards()
```

***

### Resources

* [Relayer Dashboard](https://vechain.github.io/b3tr)
* [Governance Proposal (full spec)](https://governance.vebetterdao.org/proposals/93450486232994296830196736391400835825360450263361422145364815974754963306849)
* [Discourse Proposal (design rationale)](https://vechain.discourse.group/t/vebetterdao-proposal-auto-voting-for-x-allocation-with-gasless-voting-and-relayer-rewards/559)
* [Contract Source Code](https://github.com/vechain/vebetterdao-contracts)
* [NPM Package: @vechain/vebetterdao-contracts](https://www.npmjs.com/package/@vechain/vebetterdao-contracts)
