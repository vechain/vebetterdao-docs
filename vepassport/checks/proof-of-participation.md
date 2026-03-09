# Proof of Participation

**Proof of Participation** is a module of VePassport that allows tracking the amount of times an address has used the apps of VeBetter. For every action, a score (from 0 to 400 based on the app security level) is attributed to the user interacting with the X2Earn App.

The points are then summed, generating a cumulative score for the user and if it exceeds a certain threshold, the wallet can be considered a person.&#x20;

{% hint style="danger" %}
Only the X2EarnRewardsPool contract can call the `registerAction` function, this means that if an app is not using the X2EarnRewardsPool contract to distribute their rewards then that action is never taken into account.
{% endhint %}

## Apps security levels

When an action is registered a score is being assigned to the user based on the security level of the app. The VeBetter team will be responsible for initially analyzing apps and setting the security score for each app and will open this power to the entire DAO in the future.

There are 4 security levels:

* NONE = 0 points
* LOW = 100 points
* MEDIUM = 200 points
* HIGH = 400 points

**Currently, all apps are assigned a security score of type "LOW",** therefore, for any app used, a user will receive 100 points for each action.&#x20;

## Cumulative score

Per each account, a score is calculated based on the points he gathered until now.

The formula to calculate the score takes into account 2 arguments:

* **Amount of rounds to use**: currently set to 12, which means that the contract will sum all the points gathered during the last 12 weeks;
* **Decay percentage**: currently set to 0, which reduces the points of previous rounds (eg: if set to 20%, the score calculated until the previous round will be reduced by 20% in the next, in a linear fashion).

This approach ensures that a person must continue to participate in the VeBetter ecosystem and use the apps to maintain eligibility for voting.

The following is the current implemented formula to calculate the cumulative score of the user in the last `t` amount of rounds:

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>(<code>a</code> is the total scores,  <code>r</code> is the rate of decay, <code>t</code> is number of rounds)</p></figcaption></figure>

## Threshold

The threshold an address must meet to be considered a person is **300 points accumulated during the last 12 rounds**.

## Endpoints

Other apps or contracts can interact with the Proof of Participation module through the following functions of the VePassport contract:

```solidity
interface IVeBetterPassport {
  ...
  function getCumulativeScoreWithDecay(address user, uint256 lastRound) external view returns (uint256);
  
  function userRoundScore(address user, uint256 round) external view returns (uint256);
  function userTotalScore(address user) external view returns (uint256);
  function userAppTotalScore(address user, bytes32 appId) external view returns (uint256);
  
  function thresholdPoPScore() external view returns (uint256);
  function roundsForCumulativeScore() external view returns (uint256);
  function securityMultiplier(PassportTypes.APP_SECURITY security) external view returns (uint256);
  function appSecurity(bytes32 appId) external view returns (PassportTypes.APP_SECURITY);
  ...
}
```

