# Voter Rewards

B3TR tokens are distributed weekly through VeBetter’s emissions system and sent to two separate reward pools:

* **Vote2Earn Pool**: Incentivizes all users to participate by rewarding them based on their **VOT3 token balance**
* **GM Rewards Pool** (New: [April 2025 Governance Update](https://governance.vebetterdao.org/proposals/29105190577567483236613349157882547619640909716326747339146460525546041993191)): Provides additional rewards to users who **hold a GM NFT and vote** during the cycle, with rewards based on the **reward weight** of their GM NFT level

To earn from either pool, you must cast a valid vote in a governance or allocation round during the cycle.

> All funds received by the Vote2Earn pool are used solely to reward voters proportionally based on VOT3.\
> The GM Rewards pool receives a fixed 5% of weekly emissions and distributes rewards to qualifying voters who hold a GM NFT.

## Galaxy Member NFT: Unlock Greater Rewards with Upgrades

VeBetter uses an upgradeable NFT system called the **Galaxy Member (GM) NFT** to reward committed community members. By holding a GM NFT and voting in a cycle, you become eligible to receive a portion of the **GM Rewards Pool**, which distributes 5% of weekly B3TR emissions.

The **higher your GM NFT level**, the greater your **reward weight**, determining your share of the GM pool during that voting cycle.

### Unlocking GM NFT Levels

There are **two ways** to upgrade your GM NFT:

### B3TR Requirements for Upgrades

| **Level** | **Name** | **B3TR Required** | **Reward Weight** |
| --------- | -------- | ----------------- | ----------------- |
| **2**     | Moon     | 5,000 B3TR        | 1.1               |
| **3**     | Mercury  | 12,500 B3TR       | 1.2               |
| **4**     | Venus    | 25,000 B3TR       | 1.5               |
| **5**     | Mars     | 50,000 B3TR       | 2                 |
| **6**     | Jupiter  | 125,000 B3TR      | 2.5               |
| **7**     | Saturn   | 250,000 B3TR      | 3                 |
| **8**     | Uranus   | 1,250,000 B3TR    | 5                 |
| **9**     | Neptune  | 2,500,000 B3TR    | 10                |
| **10**    | Galaxy   | 12,500,000 B3TR   | 25                |

### Benefits of Upgrading Your GM NFT

* **Maximized Rewards**:
  * Higher GM NFT levels increase your **reward weight** in the GM Rewards Pool — earn more B3TR when you vote.
* **Flexibility**:
  * Use B3TR for upgrades or enjoy free upgrades as a VeChain Node holder.
* **Dynamic Participation**:
  * Play an active role in VeBetter and get rewarded for your engagement across **both governance proposals and XAllocation votes**.

By upgrading your GM NFT and participating in governance (via voting on **governance proposal or XAllocation vote**), you unlock enhanced rewards through the **GM Rewards Pool**, which distributes 5% of weekly B3TR emissions.\
This not only boosts your personal rewards but also reinforces your long-term role in shaping the VeBetter ecosystem.

## Linear Rewards

After a successful DAO [proposal](ttps://governance.vebetterdao.org/proposals/72709653895201643147493896803002902200801427824992756263136121079630798867207) the rewards earned by participating in VeBetter governance are linear (instead of quadratic) starting from round 23, which means that you earn rewards proportionally to your VOT3 holdings at the time of the snapshot.

### Reward Formula

$$
Reward_i = \left( \frac{\sum_{v \in C} V_{i,v}}{\sum_{u} \sum_{v \in C} V_{u,v}} \right) \times Vote2Earn_{\text{cycle}}
$$

Where

* $$V_{i,v}$$ = VOT3 balance of user _**i**_ when voting on proposal _**v**_
* $$C$$ = all proposals (Governance and XAllocation Voting) in the cycle the user participated in
* $$u$$ = all users who voted on any proposal during the cycle
* $$Vote2Earn_{cycle}$$​ = total Vote2Earn reward pool for the cycle

### Example:

### Given:

* **Total participants:** 1,000 voters
* **Vote2Earn Emissions:** 1,000,000 B3TR
* **User’s VOT3 snapshot for cycle**: 10,000
* **Total VOT3 across all voters and all proposals in the cycle:** 5,300,000
* **User voted on** 1 proposal

Calculation:

$$
Reward  = (10,000 / 5,300,000) * 1,000,000 = 1,887 B3TR
$$

This voter’s 10,000 VOT3 represents **0.1887%** of the total VOT3 submitted during the cycle, earning them **1,886.79 B3TR** from the Vote2Earn reward pool.

## GM Rewards

The **GM Rewards Pool** was introduced via a successful DAO [proposal](https://governance.vebetterdao.org/proposals/29105190577567483236613349157882547619640909716326747339146460525546041993191) in **April 2025**. It distributes **5% of weekly B3TR emissions** to **Galaxy Member (GM) NFT holders** (Moon tier and above) who vote during a cycle.

Participation includes **both governance proposals and XAllocation votes** — all voting items within a cycle count.

Rewards are calculated using a **linear model** based on the user's **GM NFT level** and the **number of votes** you cast during the cycle.\
The higher your NFT level — and the more proposals you vote on — the greater your share of the GM Rewards Pool.

### Reward Formula

$$
\text{GMReward}_i = \left( \frac{\sum\limits_{v \in C} W_{i,v}}{\sum\limits_{u} \sum\limits_{v \in C} W_{u,v}} \right) \times GMRewardsPool_{\text{cycle}}
$$

Where

* $$W_{i,v}$$ = reward weight of user _**i**_ when voting on proposal _**v**_ (based on GM NFT level)
* $$C$$ = all proposals (Governance and XAllocation Voting) in the cycle the user participated in
* $$u$$ = all users who voted with a GM NFT during the cycle
* &#x20;$$GMRewardsPool_{\text{cycle}}$$  = 5% of total B3TR emissions for the cycle

### Example:

**Scenario:**

* **GM Rewards Pool this cycle:** 50,000 B3TR
* **User A:** Neptune-level GM (reward weight = 10), votes on **2 proposals**
* **User B:** Mars-level GM (reward weight = 2), votes on **2 proposals**
* **User C:** Galaxy-level GM (reward weight = 25), votes on **1 proposal**

**Total Reward Weight in the Cycle:**

* User A:   $$10×2=20$$
* User B:    $$2×2=4$$
* User C:   $$25×1=25$$
* Total Weight:  $$20+4+25=49$$

**Reward Calculations:**

**User A:**\
Reward = $${\frac{20}{49}\times 50,000 } = 20,408.16$$ B3TR

**User B:**\
Reward = $${\frac{4}{49}\times 50,000 } = 4,081.63$$ B3TR

**User C:**\
Reward = $${\frac{25}{49}\times 50,000 } = 25,510.20$$ B3TR

## Quadratic Rewarding (disabled from round 23)

Quadratic Rewarding (QR) in VeBetter is the approach used for distributing voting rewards. Under QR approach, the rewards a user receives will be determined by the square root of their VOT3 token holdings. This method ensures that while users with more tokens still receive higher rewards, the rate of increase in rewards does not grow linearly with the number of tokens held. Instead, it rises at a decreasing rate, promoting a fairer distribution of rewards and helping to close the wealth gap within our community.

