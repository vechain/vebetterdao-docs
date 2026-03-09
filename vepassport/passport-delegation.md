# Passport Delegation

Delegation allows a user to **delegate their passport personhood** to another address to allow more versatility and app interoperability. A delegator can have only one delegatee, and a delegatee can have only one delegator.

When delegating your passport, the delegatee must accept the delegation first. When a delegatee accepts the delegation his passport will become unused, giving priority to the delegated passport, regardless of the personhood check outcome.

Since by delegating the passport, the personhood check will not work anymore for the original wallet, the delegator will also lose the eligibility to vote. However, he will still need to use X2Earn apps, upgrade his GM NFT, or do whatever is needed in order for his passport to be considered a person, allowing the delegatee to vote.

An example use case for using the delegation is VeDelegate: you can have your VeWorld that you use to interact with apps with a valid Passport, that you delegate to the TBA created by VeDelegate where you deposit your B3TR tokens and automatize voting.

{% hint style="info" %}
When calling the isPerson function for a given address the contract will automatically check for delegated passports.
{% endhint %}

{% hint style="danger" %}
To avoid users misusing this feature when voting in the VeBetterDAO governance contracts the delegation will be checked upon the block number when the round starts, so new delegations will be taken into account by the B3TR governance system only after the next round starts.
{% endhint %}

## Endpoints

<pre class="language-solidity"><code class="lang-solidity">interface IVeBetterPassport {
<strong>  ...
</strong><strong>  function delegateWithSignature(address delegator, uint256 deadline, bytes memory signature) external;
</strong>  
  function delegatePassport(address delegatee) external;
  function acceptDelegation(address delegator) external;
  function denyIncomingPendingDelegation(address delegator) external;
  function cancelOutgoingPendingDelegation() external;
  
  function revokeDelegation() external;
  
  function getDelegatee(address delegator) external view returns (address);
  function getDelegator(address delegatee) external view returns (address);
  
  function isDelegator(address user) external view returns (bool);  
  function isDelegatee(address user) external view returns (bool);
  function isDelegatorInTimepoint(address user, uint256 timepoint) external view returns (bool);
  function isDelegateeInTimepoint(address user, uint256 timepoint) external view returns (bool);
  ...
}
</code></pre>
