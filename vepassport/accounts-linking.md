# Accounts Linking

To better increase the interoperability across the VeBetter ecosystem VePassport offers the possibility to link multiple addresses under a single passport. All sustainable actions performed with the linked accounts will help increment the score of the passport and better represent your onchain identity.

A **passport** is the primary identity of the user (often a user's main VeWorld Wallet), and **entities** are additional wallet addresses that a user can attach to their passport. Users can link multiple entities (e.g., hardware wallets, account abstraction wallets, regular wallets) to a single passport. Once an entity is attached, its interactions, scores, and other attributes (e.g. whitelisted, bot signals etc.) are aggregated into the passport, reflecting the combined activity.

Currently, there is a limitation of a maximum of 5 entities per passport.

When linking a wallet, the scores are not immediately transferred. Instead, once the link is established, all of the linker’s scores, from the time of the link, are credited to the linked account. After the wallet addresses are linked, only the main account — the passport address — retains the right to vote.

## Endpoints

```solidity
  interface IVeBetterPassport {
  ...
    function linkEntityToPassportWithSignature(address entity, uint256 deadline, bytes memory signature) external;
    function linkEntityToPassport(address passport) external;
    function acceptEntityLink(address entity) external;
    function removeEntityLink(address entity) external;
    function denyIncomingPendingEntityLink(address entity) external;
    function cancelOutgoingPendingEntityLink() external;
    
    function isPassport(address user) external view returns (bool);
    function isPassportInTimepoint(address user, uint256 timepoint) external view returns (bool);
    
    function isEntity(address user) external view returns (bool);
    function isEntityInTimepoint(address user, uint256 timepoint) external view returns (bool);
  ...
  }
```
