# Governance

{% hint style="info" %}
Following community proposal, since the[ 13th of August 2025](https://governance.vebetterdao.org/proposals/16456558618029822768724567372248444488437431846368752919370485354508180864756), the deposits are now counted **as voting power for XApp votes**. The deposit threshold is capped at 5M VOT3. To submit a proposal, the proposer must [**create a Discourse** ](https://vechain.discourse.group/latest)thread and be a [**Moon NFT**](../../vechain-node-and-staking-guides/legacy-vechain-node-guides-pre-stargate/vechain-nodes-x-gm-nfts-maximising-benefits-within-vebetter.md) holder.
{% endhint %}

Anyone holding VOT3 is a member of VeBetter and can take part in voting events. VOT3 is the VeBetter’s governance token, required to vote, and obtained by swapping from B3TR at a 1:1 ratio. All members can vote on proposals, participate in governance discussions, and access the Treasury, with the only minimum requirement of holding at least 1 VOT3 token. This ensures we can foster an inclusive, engaging environment for everyone.

## Governance Stabilization Period and Initialization&#x20;

At launch, VeBetter will adhere to an initial set of governance rules until it has run for one year without any technical interruptions. The Stabilization Period is critical for ensuring the long-term efficacy and viability of VeBetter. To prevent exploits during the Stabilization Period, VeChain Foundation maintains the ability to pause & restart B3TR token minting, only in the case of an exploit or other major issue. Once the requirements of the Stabilization Period have been met, community stakeholders can maintain or deprecate this functionality through governance.

## Proposal Submission&#x20;

Proposals are the backbone of any DAO, and the community is a key part of the proposal process. VeBetter will maintain a forum for proposal submission, discussion, and voting. The forum will feature a dashboard detailing voting results, historical records, and proposal details. For a proposal to reach the final stages and be implemented, it must go through a predefined process, including initial forum-based community discussion, and a community ‘temperature check,’ before on-chain voting and implementation.

### Community support

Every time a proposal is created, a deposit threshold of VOT3 tokens—calculated as a percentage of the total supply of B3TR—must be met for the proposal to become active.\
\
This deposit acts as a temperature check: if the community believes the proposal is worth considering, members can contribute VOT3. If the threshold is reached before the end of the waiting period, the proposal becomes active; otherwise, it is automatically cancelled.

Users can withdraw their VOT3 deposits once the proposal is cancelled or the voting period starts.\
While tokens are locked for supporting a proposal, they cannot be transferred or used to support other proposals during that time.&#x20;

However, **these locked VOT3 are still included in the allocation snapshot and grant the holder additional voting power in allocation rounds**. This means that if we are in Round #2 and you support a proposal that will start in Round #4, your locked tokens will still count toward your allocation voting power in Rounds #3 and #4, even though you cannot redeploy them elsewhere.

## Governance Process

The governance process is the following: **propose → vote → execute** if succeeded.&#x20;

Let's break it down:

### 1. Requirements

<figure><img src="../../.gitbook/assets/Screenshot 2025-08-18 at 11.41.23 AM.png" alt="" width="375"><figcaption></figcaption></figure>

* **Hold a GM Moon NFT**&#x20;
  * Upgrade your **Earth GM NFT** to a **GM Moon NFT**, or purchase one.
  * If you don’t hold an Earth GM NFT, you must **participate in at least one vote** to earn eligibility.<br>
* **Create a Discourse Thread**
  * Open a discussion about your proposal on the VeChain Discourse forum **at least 3 days before** submitting it.
  * This ensures community feedback before the proposal goes live.

### 2. Creating a Proposal

When drafting a proposal, include:

* **Description**: Explain what you want to achieve.
* **Action**: They are 2 type of actions possible to pass DAO proposal, Actionnable proposal ( transfer funds from the treassury for a specific purpose, and non-actionable, but propositions.&#x20;

They are 2 type of governance proposals :

There are **two types of DAO proposals**:

1.  **Actionable Proposals**

    Trigger an on-chain action, usually involving the treasury or contract calls.
2.  **Non-Actionable Proposals**

    Express ideas or directions without immediate on-chain execution.

    Even if approved, this requires off-chain development or coordination before being implemented.

Trigger an on-chain action, usually involving the treasury or contract calls.&#x20;

### 3. Proposal Process

{% stepper %}
{% step %}
### Deposit Period

During this waiting period, there is a temperature check and the community **must fund the proposal with VOT3 tokens**.  Each deposits made by the user are counted into the allocation voting as more voting power.&#x20;

Each proposal has a deposit threshold to reach, and if this threshold is not reached, then the proposal gets automatically cancelled. The proposal can be cancelled by the user who created it or by an admin of the DAO.
{% endstep %}

{% step %}
### Voting Period

Once the proposal is active, a snapshot of the total supply of VOT3 tokens and balances of each VOT3 holder is taken and used to calculate individual voting power, and VOT3 holders can vote **AGAINST/YES/ABSTAIN** for that proposal. Each proposal will remain active for the duration of the round. When you cast your vote, all 100% of your voting power will be used.
{% endstep %}

{% step %}
### Outcome vote

Once the voting period is over, if the quorum is reached and the majority voted in favour, the proposal is considered successful and can proceed to be executed. A Timelock is used for this operation, which adds a delay to proposal execution.
{% endstep %}
{% endstepper %}

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Proposal States</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Proposal Life Cycle.png" alt=""><figcaption><p>Lifecycle of the proposal</p></figcaption></figure>

### Quorum

Quorum is 30% of the total supply of VOT3. All types of votes (yes, no and abstain) are considered when calculating if quorum is reached.&#x20;

Voting rewards are distributed regardless of what type of vote was cast.

### Vote Outcome

If a quorum is not reached then the proposal is considered defeated.

If a quorum is reached then "yes" and "no" votes are counted, if "yes" votes are more than "no" votes then the proposal is considered succeeded.

Voting rewards are distributed regardless of the vote outcome.

### Execution

The VeBetter's **Governance** contract can **execute actions on all contracts deployed on the VeChainThor blockchain**, provided the respective contracts permit those actions.

We added a timelock to governance decisions, this allows users to exit the system if they disagree with a decision before it is executed. We are using OpenZeppelin’s TimelockController in combination with the GovernorTimelockControl module to achieve this.\
To execute a proposal, it first needs to be queued to Timelock contract and wait a specified amount of time before being able to execute it.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption><p>Governance setup</p></figcaption></figure>

Initially, only selected admins will be able to execute proposals.

### Vote Delegation

For DAOs that use the [Governor contract](https://docs.tally.xyz/user-guides/deploying-governor-daos/deploy-a-governor) (as we do), only delegated tokens can participate in voting. What does this mean for you, a token holder?

**You must set up delegation if you want to participate in governance votes. This includes delegating to others or voting yourself.**

If you wish to vote on proposals directly, delegate your voting power to your own address. Or, you can delegate your voting power to another community member and let them vote on your behalf.

Currently, we have an auto-delegation feature in our smart contract that will automatically delegate to you when you convert B3TR to VOT3 or someone sends you VOT3 tokens. However, this auto-delegation feature is not enabled for smart contracts, so if you are writing a smart contract that wants to receive VOT3 tokens and participate in governance you will need to manually delegate. This needs to be done only once.

## Quadratic Voting&#x20;

Quadratic Voting (QV) is a method used to re-calibrate voting power within VeBetter, specifically for the `B3TR Governance` model. Unlike traditional voting mechanisms that may disproportionately empower large token holders, QV modifies this by determining voting power based on the square root of the number of VOT3 tokens held. This approach ensures that while voting power increases with more tokens, it does so at a diminishing rate.

The formula shown below reduces the influence larger stakeholders might have, promoting a more balanced and fair decision-making process.

$$
\text{votingPower} = \sqrt{v}, \text{ where } v \text{ is the number of VOT3 tokens held}
$$

Each voter casts their vote as FOR, AGAINST, or ABSTAIN on proposals, with the effective weight of these votes directly linked to their recalculated voting power​. This structure not only aligns with standard quadratic voting practices but also ensures governance is more reflective of the wider community preferences.

### Linear Voting

By default, QV is enabled for all voting rounds. However, governance and admins can choose to disable QV, switching to linear voting. In linear voting, voting power is directly proportional to the number of VOT3 tokens a user holds.

* **Important Note**: If QV is disabled by an admin during an ongoing round, the change will only take effect starting from the next round.

## Upgradeable parameters

The following parameters can be updated by governance:

* **Quorum**: currently set at 30% of total VOT3 supply
* **Deposit Threshold**: percentage of the total supply of B3TR tokens that need to be deposited in VOT3 to create a proposal, currently set at 2%
* **Voting Threshold**: the minimum amount of tokens needed to cast a vote, currently set at 1
* **Minimum Voting Delay**: the minimum delay a proposal must be in a PENDING state before voting can start, currently set at 3 days
* **Timelock Minimum Delay**: time to wait before a proposal can be executed after it was queued
* **Voting Period**: While this value can be updated in the governance contract, it must be equal to or shorter than the Emissions Cycle. To extend the Voting Period beyond the current cycle, you must first update the Emissions Cycle duration.
* **Enable / Disable Functions Restrictions**: if disabled there isn't any restriction on what contracts and functions can be called when creating a proposal
* &#x20;**Whitelist Functions**: if function restrictions are enabled then whitelist a target and function
