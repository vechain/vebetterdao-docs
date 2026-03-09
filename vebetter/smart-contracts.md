---
description: List and description of all smart contracts of VeBetter.
---

# Smart Contracts

Contracts can be found at the following repository: [https://github.com/vechain/vebetterdao-contracts](https://github.com/vechain/vebetterdao-contracts)

The VeBetter smart contracts have undergone a comprehensive audit by [Hacken](https://hacken.io/).

{% file src="../.gitbook/assets/Hacken_Vechain Foundation_[SCA] VeChain _ VeBetter DAO _ May2024_P-2024-304_1_20240621 16_17 (1).pdf" %}

Putting it all together we get a picture of the connections of all smart contracts with hierarchies and usages between each other.

{% file src="../.gitbook/assets/vebetterdao_architecture_diagram.pdf" %}

The following is the list of all smart contracts listed alphabetically:

## B3TR

This contract governs the issuance and management of B3TR fungible tokens within the VeBetter ecosystem, allowing for minting under a capped total supply.

_Extends ERC20 Token Standard with capping, pausing, and access control functionalities to manage B3TR tokens in the VeBetter ecosystem._

`Address: 0x5ef79995FE8a89e0812330E4378eB2660ceDe699`

## B3TRProxy

This contract implements an upgradeable proxy for all of our upgradeable contracts. It is upgradeable because calls are delegated to an implementation address that can be changed. This address is stored in storage in the location specified by https://eips.ethereum.org/EIPS/eip-1967\[EIP1967], so that it doesn't conflict with the storage layout of the implementation behind the proxy.

_Forked from Openzeppelin._

## B3TRGovernor

This contract is the main governance contract for the VeBetter ecosystem. Anyone can create a proposal to both change the state of the contract, to execute a transaction on the timelock or to ask for a vote from the community without performing any on-chain action. In order for the proposal to become active, the community needs to deposit a certain amount of VOT3 tokens. This is used as a heath check for the proposal, and funds are returned to the depositors after vote is concluded. Votes for proposals start periodically, based on the allocation rounds (see xAllocationVoting contract), and the round in which the proposal should be active is specified by the proposer during the proposal creation.

A minimum amount of voting power is required in order to vote on a proposal. The voting power is calculated through the quadratic vote formula based on the amount of VOT3 tokens held by the voter at the block when the proposal becomes active.

Once a proposal succeeds, it can be executed by the timelock contract.

The contract is upgradeable and uses the UUPS pattern.

_Manages the governance process for the VeBetter ecosystem, allowing users to create and vote on proposals with VOT3 token deposits. This contract leverages OpenZeppelin's AccessControl and UUPSUpgradeable libraries for role-based access control and upgradeability. The core functionality, including proposal management, voting mechanisms, quorum calculations, and deposit logic, is modularly organized in external libraries, ensuring maintainability, flexibility, and ease of upgrades._

`Address: 0x1c65C25fABe2fc1bCb82f253fA0C916a322f777C`

## Emissions

This contract is responsible for the scheduled distribution of emissions based on predefined cycles and decay settings.

_Manages the periodic distribution of B3TR tokens to XAllocation, Vote2Earn, and Treasury allocations. This contract leverages openzeppelin's AccessControl, ReentrancyGuard, and UUPSUpgradeable libraries for access control, reentrancy protection, and upgradability._

`Address: 0xDf94739bd169C84fe6478D8420Bb807F1f47b135`

## GalaxyMember

This contract manages the unique assets owned by users within the Galaxy Member ecosystem.

_Extends ERC721 Non-Fungible Token Standard basic implementation with upgradeable pattern, burnable, pausable, and access control functionalities._

`Address: 0x93B8cD34A7Fc4f53271b9011161F7A2B5fEA9D1F`

## NodeManagement

{% hint style="warning" %}
### **Legacy contract**

This contract is part of the legacy node system. Active participation now uses StarGate staking NFTs.

[https://docs.stargate.vechain.org/](https://docs.stargate.vechain.org/?utm_source=chatgpt.com)
{% endhint %}

This contract acts as a proxy between VeChain Node Contracts and VeBetter, enabling node holders to delegate and manage multiple nodes within a single account.&#x20;

`Address: 0xB0EF9D89C6b49CbA6BBF86Bf2FDf0Eee4968c6AB`

## Timelock

This contract is used to perform the actions of the B3TRGovernor contract with a time delay. The proposers and executors roles should be assigned only to the B3TRGovernor contract.

`Address: 0x7B7EaF620d88E38782c6491D7Ce0B8D8cF3227e4`

## Treasury

This contract is designed to manage all assets owned by the VeBetter

_This contract handles the receiving and transferring of assets, leveraging upgradeable, pausable, and access control features._

`Address: 0xD5903BCc66e439c753e525F8AF2FeC7be2429593`

## VOT3

This contract governs the issuance and management of VOT3 tokens, which are the tokens used for voting in the VeBetter Ecosystem.

_Extends ERC20 Fungible Token Standard basic implementation with upgradeability, pausability, ability for gasless transactions and governance capabilities._

`Address: 0x76Ca782B59C74d088C7D2Cce2f211BC00836c602`

## VoterRewards

This contract handles the rewards for voters in the VeBetter ecosystem. It calculates the rewards for voters based on their voting power and the level of their Galaxy Member NFT.

The contract is

* upgradeable using UUPSUpgradeable.
* using AccessControl to handle the admin and upgrader roles.
* using ReentrancyGuard to prevent reentrancy attacks.
* using Initializable to initialize the contract.
* following the ERC-7201 standard for storage layout.

`Address: 0x838A33AF756a6366f93e201423E1425f67eC0Fa7`

## X2EarnApps

This contract handles the x-2-earn applications of the VeBetter ecosystem. The contract allows the insert, management, and eligibility of apps for the B3TR allocation rounds.

_The contract is using AccessControl to handle the admin and upgrader roles. Only users with the DEFAULT\_ADMIN\_ROLE can add new apps, set the base URI, and set the voting eligibility for an app. Admins can also control the app metadata and management. Each app has a set of admins and moderators (built without using AccessControl) that can manage the app metadata and management._

`Address: 0x8392B7CCc763dB03b47afcD8E8f5e24F9cf0554D`

## X2EarnCreators

This contract is designed to manage new XApp submissions within VeBetter, ensuring only verified creators can enter the ecosystem. This contract supports **Creator NFTs**, non-transferable ERC721 tokens, to grant creators access to the endorsement process.

_Extends ERC721 Non-Fungible Token Standard basic implementation with upgradeable pattern, pausable, and access control functionalities._

`Address: 0xe8e96a768ffd00417d4bd985bec9EcfC6F732a7f`

## X2EarnRewardsPool

This contract is used by x2Earn apps to reward users that performed sustainable actions. The XAllocationPool contract or other contracts/users can deposit funds into this contract by specifying the app that can access the funds.&#x20;

Admins of x2Earn apps can withdraw funds from the rewards pool, which are sent to the team wallet address. The contract is upgradeable through the UUPS proxy pattern and UPGRADER\_ROLE can authorize the upgrade.

`Address: 0x6Bee7DDab6c99d5B2Af0554EaEA484CE18F52631`

## XAllocationPool

This contract receives B3TR tokens from the Emissions contract and distributes them to the rewards pool contract and x2earn apps based on the allocation round outcome. Funds can be claimed at the end of each allocation round.

_Interacts with the Emissions contract to get the amount of B3TR available for distribution in each round, and the x2EarnApps contract to check app existence and receiver address. The contract is using AccessControl to handle roles for upgrading the contract and external contract addresses._

`Address: 0x4191776F05f4bE4848d3f4d587345078B439C7d3`

## XAllocationVoting

This contract handles the voting for the most supported x2Earn applications through periodic allocation rounds. The user's voting power is calculated on his VOT3 holdings at the start of each round, using a "Quadratic Funding" formula.

_Rounds are started by the Emissions contract. Interacts with the X2EarnApps contract to get the app data (eg: app IDs, app existence, eligible apps for each round). Interacts with the VotingRewards contract to save the user from casting a vote. The contract is using AccessControl to handle roles for admin, governance, and round-starting operations._

`Address: 0x89A00Bb0947a30FF95BEeF77a66AEdE3842Fe5B7`

## B3TR Multisig

The **B3TR Multisig** contract facilitates multisignature operations for enhanced security and collaborative management of the wallet. This contract requires approvals from multiple stakeholders before any transaction can be executed, ensuring that all actions are transparent and consensual.

This contract is not upgradable.

## Dynamic Base Allocation Pool

This contract receives surplus B3TR allocations from XAllocationPool, and was created to execute the following proposal: [https://governance.vebetterdao.org/proposals/24829756257606634701400089449417807964640877888736006394567167792281000041364](https://governance.vebetterdao.org/proposals/24829756257606634701400089449417807964640877888736006394567167792281000041364)

Initially acts as a wallet/treasury where the VeBetter team can manually distribute surplus allocations to eligible apps.

`Address: 0x98c1d097c39969bb5de754266f60d22bd105b368`

## Relayer Rewards Pool

This contract manages the reward distribution system for relayers who facilitate auto-voting functionality in the VeBetter ecosystem. Relayers perform voting actions on behalf of users who have enabled auto-voting, and earn proportional rewards based on their completed voting actions.

The pool accumulates fees collected from voter rewards when users participate through the auto-voting mechanism. Relayers can claim their proportional share of rewards based on the number of weighted voting actions they successfully complete during each round.

`Address: 0x34b56f892c9e977b9ba2e43ba64c27d368ab3c86`

## Libraries

Some contracts (eg: B3TRGovernor) store their logic inside libraries to optimize the contract size. The following is the list of libraries we created and the addresses they are deployed to.

#### GovernorClockLogic

Library for managing the clock logic as specified in EIP-6372, with fallback to block numbers.

_This library interacts with the IVOT3 interface to get the clock time or mode._

#### GovernorConfigurator

Library for managing the configuration of a Governor contract.

_This library provides functions to set and get various configuration parameters and contracts used by the Governor contract._

#### GovernorDepositLogic

Library for managing deposits related to proposals in the Governor contract.

_This library provides functions to deposit and withdraw tokens for proposals, and to get deposit-related information._

#### GovernorFunctionRestrictionsLogic

Library for managing function restrictions within the Governor contract.

_This library provides functions to whitelist or restrict functions that can be called by proposals._

#### GovernorProposalLogic

Library for managing proposals in the Governor contract.

_This library provides functions to create, cancel, execute, and validate proposals._

#### GovernorQuorumLogic

Library for managing quorum numerators using checkpointed data structures.

#### GovernorStateLogic

Library for Governor state logic, managing the state transitions and validations of governance proposals.

#### GovernorVotesLogic

Library for handling voting logic in the Governor contract.

## Contracts Upgradeability

We are using UUPS proxy for our upgradeable contracts. You can read more about it here:[https://docs.openzeppelin.com/contracts/5.x/api/proxy](https://docs.openzeppelin.com/contracts/5.x/api/proxy).

#### Resources

{% embed url="https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable" %}

<table data-header-hidden><thead><tr><th width="211"></th><th width="182"></th><th></th></tr></thead><tbody><tr><td><strong>Contract</strong></td><td><strong>Upgradable</strong></td><td><strong>Authoriser</strong></td></tr><tr><td>B3TR</td><td>No</td><td></td></tr><tr><td>B3TRProxy</td><td>No</td><td></td></tr><tr><td>B3TRGovernor</td><td>Yes</td><td>Governance OR DEFAULT_ADMIN</td></tr><tr><td>Emissions</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>GalaxyMember</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>NodeManagement (legacy)</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>Timelock</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>Treasury</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>VOT3</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>VoterRewards</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>X2EarnApps</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>X2EarnCreators</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>X2EarnRewardsPool</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>XAllocationPool</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>XAllocationVoting</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>B3TR Multisig</td><td>No</td><td></td></tr><tr><td>GrantsManager</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>RelayersRewardsPool</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>DynamicBaseAllocationPool</td><td>Yes</td><td>UPGRADER_ROLE</td></tr><tr><td>RelayerRewardsPool</td><td>Yes</td><td>UPGRADER_ROLE</td></tr></tbody></table>

#### Library updates

In `B3TRGovernor`, we are utilising libraries inside some of the modules. To upgrade the functionalities inside these libraries the following steps must be done.

1. Update library
2. Deploy new library
3. Deploy new implementation contract connecting updated library with new library address
4. Upgrade the proxy to use this new implementation smart contract using the `upgradeToAndCall` function .
