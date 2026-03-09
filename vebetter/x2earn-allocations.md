# X2Earn Allocations

The pool’s purpose is to finance sustainable apps that join the VeBetter ecosystem. The purpose of the apps is to use those tokens to reward and incentivize sustainable actions through their applications.

## Allocation rounds

Distribution of B3TR from the x-allocation pool to the apps happens in cycles, which we will refer to from now on as _rounds_. Each round will last 1 week, and funds will be distributed once the round ends.

Apps receive B3TR tokens based on the support that they receive from the VOT3 holders through voting.&#x20;

All unallocated funds of that round are sent to the Treasury contract.

<figure><img src="../.gitbook/assets/Screenshot 2025-03-28 at 10.26.22 AM.png" alt=""><figcaption></figcaption></figure>

### Rules

* **Voting Token:** The token used for voting is VOT3.
* **Voting Frequency:** Each round of voting lasts one week, though this period could change in the future.
* **Voting Eligibility:** To be able to vote, users must be verified as real persons. Each user needs to prove they have completed 3 better actions within the last 12 rounds. These actions are subject to [decay over time.](../vepassport/checks/proof-of-participation.md) A "better action" is defined as receiving a reward from an app for performing a sustainability-related activity. [Learn more here.](https://docs.vebetterdao.org/vepassport/vepassport#proof-of-participation)
* **Votes Snapshot:** The VOT3 holdings and better action are snapshotted at the start of each round. VOT3 tokens acquired and better action done after that moment won't be considered when casting the vote.
* **Voting Flexibility:** Within each round, users can vote only once but have the flexibility to allocate their votes among multiple apps. For example, a user with 100 VOT3 might assign 30 VOT3 to app #1, 20 VOT3 to app #2, and 50 VOT3 to app #3.
* **Resetting Votes:** At the beginning of each round, all previous votes are reset, requiring users to cast their votes again. This design encourages the community to return and vote regularly, hence the introduction of a "vote-2-earn" mechanism, where users are rewarded for voting.
* **Allocation Shares:** At the end of each round, the smart contract calculates the ranking and determines the distribution of funds among the apps.
* **Quorum Requirements:** To ensure broad community involvement, each round must meet a specific quorum to validate the results. If the quorum is not reached, the distribution still occurs but is based on the percentages from the last valid round.

## Voting Process

Each emission distribution starts a new allocation round for x2earn applications.

At the start of each round, a snapshot of the VOT3 holdings is made. This snapshot includes both the token held in their wallet balance and any tokens they have deposited to support proposals (if applicable).&#x20;

There can be only two possible outcomes of a round: it succeeds or fails. **To succeed the only requirement is that the quorum,** which is currently 1% of total VOT3 suppl&#x79;**, is reached.**

A user can cast his votes only when the round is active and only one time per round, by passing an array of x-application IDs and the fraction of votes he wants to allocate to each.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption><p>Example of casting a vote</p></figcaption></figure>

## Earnings calculation <a href="#allocation-and-distribution-rules" id="allocation-and-distribution-rules"></a>

* **Allocation Structure:** The distribution from the X-Allocation Pool to the apps is:&#x20;
  * 100% based on the percentage of votes each app receives since [Monday 19th 2025 proposal execution](https://governance.vebetterdao.org/proposals/79715332253006527158658105451871709430073160636438501298296079696687567453221).&#x20;
* **Cap on Allocation:** To prevent any one project from dominating, there's a cap on how much each app can receive from the 70% pool. This cap ensures that at least six projects participate in the voting each week.
* **Redistribution:** Any leftover funds from the X-Allocation Pool will be sent to the Treasury and can be redistributed through DAO voting.

Both the minimum and maximum caps can be changed by the governance.

### Quorum impact on distribution

When the quorum is not reached during an allocation voting round, it can impact the distribution process. Let's assume we have 2 rounds, where the first one succeeded and the second one did not, here is how it would work:

* **Total Funding for Current Round**: The total amount for distribution is derived from the current round, regardless of what happened in previous rounds.
  * Example: If Round 1 had 90,000 B3TR to distribute, but Round 2 had 80,000, the allocations in Round 2 will use the 80,000 units as the funding pool.
* **Base Allocation**: Every application in the current round receives a minimum allocation from the total funding pool, even if the quorum isn't met. This is calculated based on the current round's funding amount.
* **Variable Allocation**: This additional funding is determined by the voting results from the last successful round (e.g., if Round 1 succeeded, the votes from that round influence allocations in Round 2).
  * If quorum fails in the current round, variable allocations calculation will rely on the last successful round votes.
  * New applications in a failed round, with no previous votes to draw from, will only receive the base allocation.

### Distribution trigger

Distributions are not automatically sent by the contract but need to be triggered. The `claim` function is public and anyone can claim the allocation for the apps. The VeBetterDAO team however created a scheduler service to execute this action at the start of each new round.&#x20;

## Quadratic Funding

Quadratic Funding (QF) is a mechanism used by VeBetter DAO to amplify available resources and democratize fund allocation.

In this system, the proportion of the emissions pool allocated to each project is calculated by squaring the sum of the square roots of individual votes for the project and then normalizing this by the square of the total votes across all projects

&#x20;$$\text{Funding Allocation for Project }i = \left( \sum_{j} \sqrt{v_{ij}} \right)^2$$

where _j_ indexes the voters for each project

$$
\text{Normalized Funding Allocation for Project } i = \frac{\left( \sum_{j} \sqrt{v_{ij}} \right)^2}{\sum_{k} \left( \sum_{j} \sqrt{v_{kj}} \right)^2} \times \text{Emissions Pool}
$$

where _j_ indexes the voters, and _k_ indexes all projects considered in the QF model for the round.

This formula ensures that the allocation is heavily influenced by the number of distinct supporters rather than the sum of their votes alone. As a result, projects that have broader community support, regardless of the size of individual votes, receive a more significant portion of the emissions pool.

This funding model emphasizes community consensus, ensuring that projects with the widest appeal, rather than those with the most concentrated VOT3 backing, receive the greatest B3TR allocation.

### Linear Voting

By default, QF is enabled for all voting rounds. **However, admins have the option to disable QF. If QF is disabled, linear voting will be used, where the allocation is directly proportional to the number of votes received**.

* **Important Note**: If QF is disabled by an admin in the middle of a round, the change will only take effect starting from the following round.

## Upgradeable parameters

The following properties can be updated by the governance:&#x20;

* **Voting Period**: currently this equals Emission's cycles, and can never be higher than it.
* **Quorum**: percentage of VOT3 / Total supply participating in allocation voting.
* **Base Allocation Percentage**: percentage of round allocations to be divided among the apps each round, currently set at 0%.
* **Allocation Cap:** the maximum amount of votes (in percentage) that an app can receive, currently set at 20%.
