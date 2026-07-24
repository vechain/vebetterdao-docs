# RuleBook for Apps

This ruleBook is a proposal based application, to protect the sustainability and fairness of the ecosystem. 

To preserve fair distribution of weekly allocation and prevent gaming of the system, the following **rules** and **prohibitions** apply to any App seeking or receiving **B3TR incentives.**

For more context, please refer to these updates:
* [12 May 2025 Governance update](https://governance.vebetterdao.org/proposals/44152250438943884062434103573124650362722284846407941118984384695777887098183)
* [20 July 2025 Governance update](https://governance.vebetterdao.org/proposals/92759402299820464968039964071970445559062409931673742225647875754087456338977)

### 1. Direct Double Dipping <a href="#id-1-direct-double-dipping" id="id-1-direct-double-dipping"></a>

Apps must not use their own rewards to incentivize behaviors already directly compensated by protocol-level Vote2Earn incentives (e.g., proposal voting).

### 2. Recursive Rewarding (Incentive Layering) <a href="#id-2-recursive-rewarding-incentive-layering" id="id-2-recursive-rewarding-incentive-layering"></a>

Apps must not reward users for interacting with another incentivized App when:

* The action is already subject to B3TR allocation.
* The behavior results in redundant compensation for the same activity.

This includes:

* Wrapping or mirroring existing Apps to farm additional incentives.
* Stacking reward layers for a single on-chain action.
* Reciprocal arrangements between Apps that loop rewards for shared behaviors.

### 3. Fragmentation for Reward Multiplication <a href="#id-3-fragmentation-for-reward-multiplication" id="id-3-fragmentation-for-reward-multiplication"></a>

Splitting functionalities into multiple Apps for the purpose of inflating total allocations is not allowed. Interdependent or co-deployed Apps may be evaluated as a unit.

### 4. Social Amplification Loops <a href="#id-4-social-amplification-loops" id="id-4-social-amplification-loops"></a>

Rewarding users for promoting, relaying, or proxying already-incentivized actions (e.g., vote sharing or vote proxying) is disallowed if the base action is already rewarded by the protocol.

### 5. In-app Governance <a href="#id-5-in-app-governance" id="id-5-in-app-governance"></a>

#### 5.1. User-Affirmed Governance & UI
* **User Confirmation**: Any modification of user vote preferences must be executed through a clear, user-signed transaction. Background updates without a dedicated user-confirmed transaction are prohibited.
* **Visibility of Choice**: The UI must display all endorsed Apps, ensuring users can modify selections for any project before confirmation.
* **Opt-Out Requirement**: Apps must provide a visible UI component to opt out of the governance service at any time.
* **Conversion Allowance**: Apps may include a function to convert B3TR to VOT3 when a user initiates in-app voting as long as such transaction is disclosed and confirmed by the user.

#### 5.2. Self-Promotion
Apps may prioritize themselves in the interface and pre-fill vote preferences as follows:
* **New Users**: If no existing preferences exist, the App may pre-fill with 100% weight toward itself.
* **Existing Users**: The App may add itself to the list, provided its pre-filled weight is lower than or equal to any individual App in the user’s new selection.

#### 5.3. True Neutrality
If an App provides governance services and the user opts in without specifying preferences, or preferences become ineligible, the following defaults apply:
* **App Allocation**: No vote shall be cast until the user explicitly confirms a selection through a signed transaction.
* **Proposal Voting**: DAO-wide proposals must be set to “Abstain.”

#### 5.4. Vote Redistribution
* **Weight Redistribution**: If an App in a user’s preference becomes ineligible, its weight must be redistributed equally among the user’s remaining selected Apps.
* **Redirection Prohibition**: Apps cannot redirect weight from ineligible projects to the host App or any project not explicitly included in the user’s most recent confirmed selection. If all projects become ineligible in the user’s selection, Rule 5.3 applies.
* **Vote Modification**: Apps are prohibited from modifying a user’s existing vote preferences, including the addition or removal of eligible projects, except where explicitly required by the redistribution logic defined in Rule 5.4 (Weight Redistribution and Redirection Prohibition), and according to Self-Promotion described in Rule 5.2.

**Exemption**: Rules under section 5 apply to all in-app voting integrations unless limited by the functional logic of the native VBD auto-voter, as its automation and logic are subject to DAO governance and community contributions.

### 6. Anti-Bribery Measures <a href="#id-6-anti-bribery-measures" id="id-6-anti-bribery-measures"></a>

Apps may not offer manual rewards, “x2earn” multipliers, or any perks that assist in unlocking them if such incentives are conditioned on the user voting for that specific App.

### 7. Capital Integrity <a href="#id-7-capital-integrity" id="id-7-capital-integrity"></a>

Apps are prohibited from using allocation or grant funding received from the VeBetter DAO Treasury to vote in weekly App allocation rounds.

**Exemption**: Apps maintain the right to use Treasury grant funds and allocation to support and vote on proposals.

TLDR;

* Rewarding users beyond the App action-based incentives is strictly prohibited.
* No extra rewards for actions already incentivized by VeBetter are allowed. 
* In-app governance must be user-affirmed, transparent, and neutral.
* Bribing users for votes, as well as using Treasury grant funding or App allocation to vote in weekly allocation rounds, is prohibited.



<br>
