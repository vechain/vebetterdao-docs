# Manage distributors funds

Every app receives B3TR from weekly allocation rounds in a contract called X2EarnRewardsPool and those rewards are now configurable by the app admin. In fact, you can split allocated tokens between **distributable rewards pool** for your user and a **treasury pool**, withdrawal for operational needs. You also have the possibility to **pause distribution** anytime, this won't affect the reward pool unless you move funds to the app balance.&#x20;

This **dual-pool mechanism** allows you to have more control over your distribution funds.\
\
You can manage these configurations directly from the **app admin dashboard** in the VeBetter app. Or by interacting with our **`X2EarnRewardsPool` contract.**&#x20;

{% hint style="warning" %}
Only the **app admin** can set this **distributable rewards pool,** by increasing/ decreasing the wished amount. \
\
If you are **new to the DAO**, note that this feature will be enabled by default. You will have to **immediately refill your rewards pool**, up to the amount you want to distribute.
{% endhint %}

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F5gLJKT3UdlvblZqAiaZ2%2Fuploads%2FNcjwDzd4KWvUcx7ZcIii%2FDEMO-locking-mechanism.mp4?alt=media&token=a726789b-9e53-42a8-9c0c-99c953732a0d" %}

### Q\&A&#x20;

<details>

<summary>What happen if my rewards pool goes to 0 ?</summary>

If your balance is empty, it's either because you enabled the feature without adding funds to your rewards pool, or because you've run out of funds. You can either **refill** your rewards pool or **disable** the feature entirely at any time:

**Via VeBetterDAO Dashboard**

* **Refill**: Add funds directly through the VBD dashboard
* **Disable**: Toggle off the feature in settings

**Via** `X2EarnRewardsPool`**Smart Contract**&#x20;

<pre class="language-solidity"><code class="lang-solidity">// Option 1: Refill your rewards pool with additional funds
// @param amount The amount of tokens to add to your rewards pool
X2EarnRewardsPool.increaseRewardsPoolBalance(bytes32 appId, uint256 amount)


// Option 2: Toggle rewards functionality on/off
// @param enable Set to false to disable rewards, true to enable
<strong>X2EarnRewardsPool.toggleRewardsPoolBalance(bytes32 appId, bool enable)
</strong></code></pre>

</details>

