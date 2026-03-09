# Solidity

In this example, we assume that you are building a smart contract where users can submit some sustainable action that is being approved/rejected by some moderator. If the action is approved the user can claim his rewards. No backend is involved.

Your contract should look like this:

<pre class="language-solidity"><code class="lang-solidity"><strong>// SPDX-License-Identifier: MIT
</strong>pragma solidity 0.8.20;

// can find here: https://github.com/vechain/vebetterdao-contracts/tree/main/contracts/interfaces
import "./interfaces/IX2EarnRewardsPool.sol";

/**
 * Example contract with key functionality to interact with the x2Earn rewards pool
 * and distribute rewards.
 * You can customize this contract in the way 
 */
contract MySustainableAppContract {
    IX2EarnRewardsPool public x2EarnRewardsPool;
    bytes32 public VBD_APP_ID;
    
    // a dummy mapping pretending you have a way to track and 
    // validate user actions on chain.
    mapping(uint256 actionId => ActionStruct) public sustainableActions;
    mapping(uint256 actionId => bool) public rewardsClaimed;
    
    // You will need to setup the address of the X2EarnRewardsPool contract
    // and your APP_ID on VeBetterDAO
    constructor(IX2EarnRewardsPool _x2EarnRewardsPool, bytes32 _VBD_APP_ID) {
        x2EarnRewardsPool = _x2EarnRewardsPool;
        VBD_APP_ID = _VBD_APP_ID;
    }

    /**
     * @notice A function that allows a user to claim a reward for a specific
     * sustainable action that they performed.
     * 
     * IMPORTANT: the address of this contract must be set as a distributor 
     * of your APP in order to move funds from the X2EarnRewardsPool contract. 
     * You can set this on the VeBetterDAO governance app.
     */
    function claimReward(uint256 _actionId) external {
        // ... some code to check if the action is valid and user can claim

        // If distributeReward fails, it will revert
        x2EarnRewardsPool.distributeReward(
            VBD_APP_ID,
            actions[_actionId].rewardAmount,
            msg.sender, // this is the user calling the claimReward function
            "" // proof and impacts not provided
        );

        rewardClaimed[_actionId] = true;

        emit RewardClaimed(_actionId, msg.sender);
    }
}
</code></pre>

{% hint style="warning" %}
The address of this contract must be set as a **distributor** of your APP in order to move funds from the X2EarnRewardsPool contract.



You can set this on the VeBetter governance app.

![](<../../.gitbook/assets/image (27).png>)
{% endhint %}

{% hint style="info" %}
### Sustainability Proof

Read more about the proof standard and how we expect you to provide it in the [Sustainability Proofs and Impacts](../sustainability-proof-and-impacts.md) section.
{% endhint %}

{% hint style="danger" %}
To be able to distribute the rewards you will need to add the PUBLIC ADDRESS of the wallet calling the `distributeRewards` function as a Reward Distributor of your app. \
\
To do so, you need to:

1\) Connect with your app's admin wallet to the governance dapp

2\) Go to your app's page

3\) Click the _cogs_ button to enter the settings page

4\) Scroll down to the "Reward Distributors" section, and add the public address as a reward distributor

5\) Save changes
{% endhint %}

<figure><img src="../../.gitbook/assets/SCR-20250502-osad.png" alt=""><figcaption></figcaption></figure>
