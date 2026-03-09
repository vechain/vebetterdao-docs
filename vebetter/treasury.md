# Treasury

The Treasury is the beating heart of any DAO, representing the store of wealth and source of funds for the ecosystem. Adhering to the core principles of Web3, the VeBetter DAO Treasury will be transparent, fiscally efficient, and auditable.&#x20;

Treasury management follows the rules and protocols defined in VeBetter Governance. The uncompromising principle of the Treasury Pool is that tokens will only be used for expanding and building the VeBetter ecosystem, making the world better in the process.

## Implementation

Treasury will hold all assets belonging to the DAO. Such assets include `VET`, `VTHO`, `B3TR`, `VOT3` and any other `ERC-20` or `ERC-721` tokens that have been transferred to the Treasury contract address.

Tokens can only be transferred from the Treasury to another address via a governance proposal. The same goes for converting to `B3TR`/ `VOT3` .

Token balances can be viewed by anyone by calling one of the balance functions.

In the case that the Treasury needs to stop all transactions, the smart contract can be paused.

This contract cannot transfer tokens out that are non-native or non ERC-721 or ERC-20.

## Contract Functionality <a href="#id-2.-contract-functionality" id="id-2.-contract-functionality"></a>

| **Function**         | **Access Control** | **Description**                                                                                                |
| -------------------- | ------------------ | -------------------------------------------------------------------------------------------------------------- |
| `pause()`            | Contract Admin     | Pause contract so no transfers can be carried out                                                              |
| `upgradeToAndCall()` | Upgrader Role      | Upgrade implementation contract                                                                                |
| `transferVTHO()`     | Governance         | Withdraw VTHO from treasury                                                                                    |
| `transferB3TR()`     | Governance         | Withdraw B3TR from treasury                                                                                    |
| `transferVOT3()`     | Governance         | Withdraw VOT3 from treasury                                                                                    |
| `transferVET()`      | Governance         | Withdraw VET from treasury                                                                                     |
| `transferTokens()`   | Governance         | Withdraw any erc-20 token from treasury, address needs to be passed in so that contract can be interacted with |
| `transferNFT()`      | Governance         | Withdraw any NFT from treasury address needs to be passed in so that contract can be interacted with           |
| `convertB3TR()`      | Governance         | Converts B3TR for VOT3                                                                                         |
| `convertVOT3()`      | Governance         | Converts VOT3 to B3TR                                                                                          |

### Transfer Limits

Each token has a transfer limit in place to avoid moving huge sums of money in a single operation. Those limits can be updated by governance and are token-specific.

Current limits are:

* 200,000 VET
* 200,000 B3TR
* 3,000,000 VTHO
* 50,000 VOT3
