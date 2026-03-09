# Personhood Check

The **Personhood Check** refers to a function provided by the contract to determine whether a particular wallet belongs to a real person or not.&#x20;

```solidity
interface IVeBetterPassport {
  ...
  function isPerson(address user) external view returns (bool person, string memory reason);
  function isPersonAtTimepoint(
    address user,
    uint48 timepoint
  ) external view returns (bool person, string memory reason)
  ...
}
```

The contracts currently calling this function are those related to governance voting in **VeBetter**, such as `XallocationVoting` and `B3TRGovernor`. When this function is called, the VePassport contract checks the following criteria:

* If the address (representing the user) has delegated their passport, their personhood is not considered, meaning they will not be regarded as a person;&#x20;
* If the address is whitelisted, it will be considered a person;&#x20;
* If the address is blacklisted, they will not be considered a person;&#x20;
* If an address has been flagged multiple times by one or more apps as a bot or as an account involved in a Sybil attack, it will not be considered a person;&#x20;
* If the address has used apps enough in previous rounds, or in the current one, to exceed the participation threshold, they will be considered a person;&#x20;
* If the user holds a **GM NFT** with a level of 2 or higher, they will be considered a person;&#x20;
* If none of the above happens, the user will not be considered a person.

## Enabled checks

Not all of the previously listed checks are enabled. Those checks can be enabled or disabled as desired, and currently, only the **delegation** and **Proof of Participation** checks are enabled.&#x20;

At the moment the checks can be enabled by the VeBetter team, but in the future, this option will also be open to the DAO itself.

The rules governing how this method works can be adjusted over time.

## Check personhood in a specific block

In addition to determining whether a wallet is considered a person at the current moment, it is also possible to verify if a wallet is considered a person at a specific block in the past. This function is particularly used by the governance contracts of **VeBetter** during voting, where it checks if a user was considered a person at the start of the round.&#x20;

Not all checks support this functionality though. Currently only the Delegation and Proof of Investment support this. All other checks, such as whitelisting, blacklisting, bot signaling, and participation, are analyzed based on the current block.
