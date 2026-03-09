---
description: >-
  Roles define the permissions and access levels across different contracts and
  modules. This page outlines the structure of roles, their functions, and the
  path toward full decentralization.
---

# Roles And Permissions

To maintain a secure and flexible smart contract system, we leverage roles to manage administrative tasks and define who has the authority to perform specific actions. These roles control everything from minting tokens to upgrading contracts, pausing operations, and setting governance parameters.

The central goal of this structure is to gradually decentralize power from individual wallets to the DAO (the B3TRGovernor governance contract).

## Roles

This document categorizes roles and outlines their functionalities, detailing what permissions each role grants. Here's a quick summary of some common roles and what they do:

* **DEFAULT\_ADMIN\_ROLE**: The superuser role with the ability to grant, revoke, and renounce roles.
* **PAUSER\_ROLE**: Controls the ability to pause and unpause contract functionality.
* **MINTER\_ROLE**: Allows for token minting.
* **UPGRADER\_ROLE**: Permits contract upgrades and modifications.
* **CONTRACTS\_ADDRESS\_MANAGER\_ROLE**: Manages critical contract addresses.
* **DECAY\_SETTINGS\_MANAGER\_ROLE:** Manages parameters surrounding cycle lengths, allocation percentages and decay rates in the Emission contract.&#x20;
* **GOVERNANCE\_ROLE**: Involved in governance decisions, proposal management, and voting.

All roles have in common the following functionality: they can renounce that role.

Once there isn’t any address that has the DEFAULT\_ADMIN\_ROLE then it won’t be possible to grant or revoke any role. If some contract is upgradeable and the DAO has the UPGRADER\_ROLE then the DAO could decide to upgrade the contract and assign the role to whoever they want.

## Permissions

The ultimate aim is to create a self-governing system where no single entity holds absolute control. This section of the page provides a detailed plan for achieving this goal, outlining the steps for transitioning roles to the B3TRGovernor contract, removing direct admin control, and enabling decentralized governance.

Follow the outlined steps to understand the current state of role assignments and the specific actions required to achieve a more autonomous system. Through this journey, we aim to establish a resilient and community-driven ecosystem where the power of decision-making rests in the hands of the DAO and its stakeholders.

## Contract: B3TR

| Role Name            | Functions                           | Function Descriptions                                    | Initial Assignees                    |
| -------------------- | ----------------------------------- | -------------------------------------------------------- | ------------------------------------ |
| DEFAULT\_ADMIN\_ROLE | grantRole, revokeRole, renounceRole | Grants or revokes roles; allows renunciation of its role | Admin Multi Signature                |
| PAUSER\_ROLE         | Pause, Unpause                      | Pauses or unpauses contract operations                   | ~~Admin Wallet~~                     |
| MINTER\_ROLE         | Mint                                | Mints new tokens                                         | ~~Admin Wallet~~, Emissions Contract |

#### **Steps for full decentralization:**

* [x] After we migrate to mainnet we revoke the MINTER\_ROLE from the admin wallet.
* [x] When we no longer want the power to pause the contract we revoke the PAUSER\_ROLE.
* [ ] We renounce the DEFAULT\_ADMIN\_ROLE

## Contract: B3TRGovernor

<table data-header-hidden><thead><tr><th width="193"></th><th width="206"></th><th></th><th></th></tr></thead><tbody><tr><td>Role Name</td><td>Functions</td><td>Function Descriptions</td><td>Initial Assignees</td></tr><tr><td>DEFAULT_ADMIN_ROLE</td><td><p>grantRole, revokeRole, renounceRole, cancel,</p><p>_authorizeUpgrade, relay, setDepositThreshold, setVotingThreshold, setMinVotingDelay, updateQuorumNumerator</p></td><td>Grants or revokes roles; allows renunciation of its role; cancel pending proposals;</td><td>Admin Wallet</td></tr><tr><td>PROPOSAL_EXECUTOR_ROLE</td><td>Execute</td><td>Executes queued proposals</td><td>Admin Wallet</td></tr><tr><td>PAUSER_ROLE</td><td>Pause, Unpause</td><td>Pauses or unpauses contract (no proposal creation, queuing, or execution)</td><td>Admin Wallet</td></tr><tr><td>GOVERNOR_FUNCTIONS_SETTINGS_ROLE</td><td>setWhitelistFunction, setWhitelistFunctions, setIsFunctionRestrictionEnabled</td><td>Whitelist function that people can trigger in their proposals, or disable this check.</td><td>Admin Wallet, BTRGovernor</td></tr><tr><td>CONTRACTS_ADDRESS_MANAGER_ROLE</td><td>setVoterRewards, setXAllocationVoting, updateTimelock</td><td>Manages contract addresses</td><td>Admin Wallet, BTRGovernor</td></tr></tbody></table>

#### **Steps for full decentralization:**

* [ ] B3TRGovernor is already responsible for upgrading, changing voting or deposit threshold, min voting delay, and quorum.
* [ ] We grant the GOVERNOR\_FUNCTIONS\_SETTINGS\_ROLE to the B3TRGovernor, and revoke it from the current wallet. Now governance can vote on whitelisting other functions or completely remove whitelisting of functions on proposal creation.
* [ ] We grant the CONTRACTS\_ADDRESS\_MANAGER\_ROLE to the B3TRGovernor, and revoke it from the current wallet.
* [ ] We renounce the PAUSER\_ROLE or grant it to the B3TRGovernor
* [ ] We renounce the DEFAULT\_ADMIN\_ROLE and PROPOSAL\_EXECUTOR\_ROLE

## Contract: Emissions

| Role Name                         | Functions                                                                                      | Function Descriptions                                     | Initial Assignees       |
| --------------------------------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------- | ----------------------- |
| MINTER\_ROLE                      | Bootstrap, Start, renounceRole                                                                 | Allows minting of tokens, initializing processes          | ~~Admin Wallet~~        |
| DEFAULT\_ADMIN\_ROLE              | grantRole, revokeRole, renounceRole                                                            | Manages roles and contract parameters                     | Admin multisig contract |
| DECAY\_SETTINGS\_MANAGER\_ROLE    | setCycleDuration, setXAllocationsDecay, setVote2EarnDecay                                      | Sets emission cycle durations and related parameters      | Admin Wallet            |
|                                   | setXAllocationsDecayPeriod, setVote2EarnDecayPeriod                                            | Sets decay periods for various emission-related settings  |                         |
|                                   | setTreasuryPercentage, setScalingFactor, setMaxVote2EarnDecay                                  | Configures emission scaling factors and treasury settings |                         |
| CONTRACTS\_ADDRESS\_MANAGER\_ROLE | setXAllocationAddress, setVote2EarnAddress, setTreasuryAddress, setXAllocationsGovernorAddress | Configures addresses for key contracts                    | Admin Wallet            |
| UPGRADER\_ROLE                    | upgrade, renounceRole                                                                          | Allows contract upgrades                                  | ~~Admin Wallet~~        |

#### **Steps for full decentralization:**

* [x] We renounce the MINTER\_ROLE once we complete the mainnet migration
* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the DEFAULT\_ADMIN\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the CONTRACTS\_ADDRESS\_MANAGER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the **DECAY\_SETTINGS\_MANAGER\_ROLE** to the B3TRGovernor and revoke it from the current wallet

## Contract: GalaxyMember

| Role Name                         | Functions                                                                                        | Function Descriptions                                                          | Initial Assignees |
| --------------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ | ----------------- |
| DEFAULT\_ADMIN\_ROLE              | setMaxLevel, setMaxMintableLevels, setBaseURI, setB3TRtoUpgradeToLevel, setIsPublicMintingPaused | Sets contract parameters for levels, minting, and URI, Controls public minting | Admin Wallet      |
| UPGRADER\_ROLE                    | Upgrade                                                                                          | Allows contract upgrades                                                       | Admin Wallet      |
| PAUSER\_ROLE                      | Pause, Unpause                                                                                   | Pauses and unpauses the contract                                               | Admin Wallet      |
| MINTER\_ROLE                      | Mint                                                                                             | Mint tokens for migration purposes                                             | Admin Wallet      |
| CONTRACTS\_ADDRESS\_MANAGER\_ROLE | setXAllocationsGovernorAddress, setB3trGovernorAddress                                           | Configures critical contract addresses                                         | Admin Wallet      |

#### **Steps for full decentralization:**

* [x] We renounce the MINTER\_ROLE once we complete the mainnet migration
* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the CONTRACTS\_ADDRESS\_MANAGER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We renounce the PAUSER\_ROLE
* [ ] We grant the DEFAULT\_ADMIN\_ROLE to the B3TRGovernor and revoke it from the current wallet

## Contract: Timelock

| Role Name            | Functions                           | Function Descriptions                                        | Initial Assignees               |
| -------------------- | ----------------------------------- | ------------------------------------------------------------ | ------------------------------- |
| DEFAULT\_ADMIN\_ROLE | grantRole, revokeRole, renounceRole | Grants or revokes roles; allows renunciation of its own role | Admin Wallet, Timelock contract |
| Proposer             | schedule, cancel                    | Proposes, schedules, and cancels transactions                | B3TRGovernor                    |
| Executor             | Execute                             | Executes approved proposals and transactions                 | B3TRGovernor                    |
| UPGRADER\_ROLE       | upgrade                             | Allows contract upgrades                                     | Admin Wallet                    |

#### **Steps for full decentralization:**

* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from current wallet
* [ ] We renounce the DEFAULT\_ADMIN\_ROLE and now Timelock is self-administrating, which means that if the DAO wants to change the proposer or executor they will need to create a proposal that calls the Timelock contract to grant/revoke that role

## Contract: Treasury

| Role Name            | Functions                                             | Function Descriptions                           | Initial Assignees |
| -------------------- | ----------------------------------------------------- | ----------------------------------------------- | ----------------- |
| DEFAULT\_ADMIN\_ROLE | grantRole, revokeRole, renounceRole                   | Grants/revokes roles; can renounce its own role | Admin Wallet      |
| GOVERNANCE\_ROLE     | transferVTHO, transferB3TR, transferVOT3, transferVET | Manages VOT3, B3TR, VET and VTHO transfers      | B3TRGovernor      |
|                      | transferTokens, transferNFT                           | Manages NFT and ERC20 token transfers           |                   |
|                      | stakeB3TR, unstakeB3TR                                | Manages staking and unstaking                   |                   |
| PAUSER\_ROLE         | Pause, Unpause                                        | Pauses and unpauses contract operations         | Admin Wallet      |
| UPGRADER\_ROLE       | Upgrade                                               | Allows contract upgrades                        | Admin Wallet      |

#### **Steps for full decentralization:**

* [ ] The B3TRGovernor already has the GOVERNANCE\_ROLE, if we have some admin that has this role then we need to renounce it
* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We renounce the PAUSER\_ROLE
* [ ] We can grant the DEFAULT\_ADMIN\_ROLE to the B3TRGovernor or renounce it

## Contract: VOT3

| Role Name            | Functions                           | Function Descriptions                                    | Initial Assignees |
| -------------------- | ----------------------------------- | -------------------------------------------------------- | ----------------- |
| DEFAULT\_ADMIN\_ROLE | grantRole, revokeRole, renounceRole | Grants or revokes roles; allows renunciation of its role | Admin Wallet      |
| UPGRADER\_ROLE       | Upgrade                             | Allows contract upgrades                                 | Admin Wallet      |
| PAUSER\_ROLE         | Pause, Unpause                      | Pauses or unpauses contract operations                   | Admin Wallet      |

#### **Steps for full decentralization:**

* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet.
* [ ] When we no longer want the power to pause the contract we revoke the PAUSER\_ROLE.
* [ ] We renounce the DEFAULT\_ADMIN\_ROLE role.

## Contract: VoterRewards

| Role Name                         | Functions                              | Function Descriptions                        | Initial Assignees               |
| --------------------------------- | -------------------------------------- | -------------------------------------------- | ------------------------------- |
| DEFAULT\_ADMIN\_ROLE              | setLevelToMultiplier, setScalingFactor | Sets reward scaling factors and levels       | Admin Wallet                    |
| UPGRADER\_ROLE                    | Upgrade                                | Allows contract upgrades                     | Admin Wallet                    |
| VOTE\_REGISTRAR\_ROLE             | registerVote                           | Registers user votes for rewards             | XAllocationVoting, B3TRGovernor |
|                                   | setVoteRegistrarRole                   | Assigns who has permission to register votes |                                 |
| CONTRACTS\_ADDRESS\_MANAGER\_ROLE | setEmissions, setGalaxyMember          | Sets key contract addresses                  | Admin Wallet                    |

#### **Steps for full decentralization:**

* [ ] VOTE\_REGISTRAR\_ROLE should be assigned only to the contracts performing voting
* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the CONTRACTS\_ADDRESS\_MANAGER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the DEFAULT\_ADMIN\_ROLE to the B3TRGovernor and revoke it from current wallet

## Contract: X2EarnApps

| Role Name            | Functions                          | Function Descriptions                          | Initial Assignees |
| -------------------- | ---------------------------------- | ---------------------------------------------- | ----------------- |
| DEFAULT\_ADMIN\_ROLE | setBaseURI, UpdateAppMetadata      | Configures base URI and updates app metadata   | Admin Wallet      |
|                      | Update receiver address/ app admin | Sets or updates app receiver address and admin |                   |
|                      | Add/Remove app moderators          | Manages app moderators                         |                   |
| UPGRADER\_ROLE       | Upgrade                            | Allows contract upgrades                       | Admin Wallet      |
| GOVERNANCE\_ROLE     | Add app, setVotingEligibility      | Adds new apps and defines voting eligibility   | Admin Wallet      |

#### **Steps for full decentralization:**

* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the GOVERNANCE\_ROLE to the B3TRGovernor and revoke it from current wallet
* [ ] We grant the DEFAULT\_ADMIN\_ROLE to the B3TRGovernor and revoke it from current wallet

**NB**: This contract will be upgraded to handle app endorsement, so the “Add app” functionality triggered by a GOVERNANCE\_ROLE could be removed in a future release

## Contract: X2EarnRewardsPool

| Role Name                         | Functions     | Function Descriptions                     | Initial Assignees |
| --------------------------------- | ------------- | ----------------------------------------- | ----------------- |
| DEFAULT\_ADMIN\_ROLE              |               |                                           | Admin Wallet      |
| UPGRADER\_ROLE                    | Upgrade       | Allows contract upgrades                  | Admin Wallet      |
| CONTRACTS\_ADDRESS\_MANAGER\_ROLE | setX2EarnApps | Update the contract address of X2EarnApps | Admin Wallet      |

#### **Steps for full decentralization:**

* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the CONTRACTS\_ADDRESS\_MANAGER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We renounce the DEFAULT\_ADMIN\_ROLE role

## Contract: XAllocationPool

| Role Name                         | Functions                                                                                   | Function Descriptions               | Initial Assignees |
| --------------------------------- | ------------------------------------------------------------------------------------------- | ----------------------------------- | ----------------- |
| DEFAULT\_ADMIN\_ROLE              | grantRole, revokeRole, renounceRole                                                         | Grants, revokes, or renounces roles | Admin Wallet      |
| UPGRADER\_ROLE                    | Upgrade                                                                                     | Allows contract upgrades            | Admin Wallet      |
| CONTRACTS\_ADDRESS\_MANAGER\_ROLE | setXAllocationVotingAddress, setEmissionsAddress, setTreasuryAddress,  setX2EarnAppsAddress | Manages key contract addresses      | Admin Wallet      |

#### **Steps for full decentralization:**

* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the CONTRACTS\_ADDRESS\_MANAGER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the DEFAULT\_ADMIN\_ROLE to the B3TRGovernor and revoke it from current wallet

## Contract: XAllocationVoting

| Role Name                         | Functions                                                         | Function Descriptions                      | Initial Assignees          |
| --------------------------------- | ----------------------------------------------------------------- | ------------------------------------------ | -------------------------- |
| DEFAULT\_ADMIN\_ROLE              | grantRole, revokeRole, renounceRole                               | Grants, revokes, or renounces roles        | Admin Wallet               |
| UPGRADER\_ROLE                    | Upgrade                                                           | Allows contract upgrades                   | Admin Wallet               |
| ROUND\_STARTER\_ROLE              | startNewRound                                                     | Initiates a new voting round               | Emissions Contract         |
| GOVERNANCE\_ROLE                  | setVotingThreshold, setAppSharesCap                               | Sets parameters for voting                 | Admin Wallet, B3TRGovernor |
|                                   | setBaseAllocationPercentage, setVotingPeriod                      | Establishes voting periods and percentages |                            |
|                                   | updateQuorumNumerator                                             | Updates quorum requirements                |                            |
| CONTRACTS\_ADDRESS\_MANAGER\_ROLE | setX2EarnAppsAddress, setEmissionsAddress, setVoterRewardsAddress | Manages key contract addresses             | Admin Wallet               |

#### **Steps for full decentralization:**

* [ ] ROUND\_STARTER\_ROLE should always be the Emissions contract
* [ ] Renaunce GOVERNANCE\_ROLE
* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the CONTRACTS\_ADDRESS\_MANAGER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the DEFAULT\_ADMIN\_ROLE to the B3TRGovernor and revoke it from current wallet

## Contract: Dynamic Base Allocation Pool

| Role Name            | Functions                           | Function Descriptions               | Initial Assignees |
| -------------------- | ----------------------------------- | ----------------------------------- | ----------------- |
| DEFAULT\_ADMIN\_ROLE | grantRole, revokeRole, renounceRole | Grants, revokes, or renounces roles | Admin Wallet      |
| UPGRADER\_ROLE       | Upgrade                             | Allows contract upgrades            | Admin Wallet      |
| DISTRIBUTOR\_ROLE    | distributeDBARewards                | Sends to surplus allocation to apps | Admin Wallet      |

#### **Steps for full decentralization:**

* [ ] We grant the UPGRADER\_ROLE to the B3TRGovernor and revoke it from the current wallet
* [ ] We grant the DEFAULT\_ADMIN\_ROLE to the B3TRGovernor and revoke it from current wallet
* [ ] We update the contract to filter eligible apps onchain avoiding where everyone can trigger the distribution function and we can get rid of the DISTRIBUTOR\_ROLE
