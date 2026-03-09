---
description: >-
  Learn how to automate your weekly voting and reward claiming. A relayer
  service handles all transactions for you with a 10% fee (capped at 100 B3TR)
  to cover gas costs.
---

# Automation

## Auto-Voting & Auto-Claiming Rewards

{% hint style="success" %}
**MVP Phase**: Automation is currently in its MVP phase. Features, requirements, and fee structures are subject to change based on user feedback and operational requirements.
{% endhint %}

### Overview

This feature allows you to automate your weekly voting and reward claiming process. Once enabled, a relayer service will automatically cast your votes and claim your rewards on your behalf each week.

### Getting Started

1. Navigate to the [**allocations**](https://governance.vebetterdao.org/allocations) **page**
2. Ensure you meet all eligibility [requirements](https://docs.vebetterdao.org/vebetter/automation#whats-required-to-enable)
3. Select your preferred apps for auto-voting
4. **Toggle-on** Auto-voting & claim
5. That's it! Your votes and rewards will be handled starting from next week onwards.

<div data-full-width="false" data-with-frame="true"><figure><img src="../.gitbook/assets/governance.vebetterdao.org_allocations(iPhone 14 Pro Max).png" alt="" width="375"><figcaption></figcaption></figure></div>

### What’s Required to Enable

To use this service, you must meet **all** of the following criteria at the time of snapshot:

* **Hold at least 1 VOT3 token** in your wallet
* **Complete 3 sustainable actions** before the snapshot
* **Not be flagged as a bot** by any App owner
* **Have selected at least one eligible app** for voting

### How It Works

#### Note for First-Time Activation

If you enable automation **during the current round**, the service will kick in **from the next round onwards**.

**What this means:**

* You’ll still need to **manually claim your rewards** when the current round ends.

#### Weekly Automation Cycle

1. **Snapshot**: At the start of each voting round, a snapshot is taken to determine eligible participants
2. **Auto-Voting**: The service automatically casts votes based on your selected app preferences
3. **Auto-Claiming**: After the round ends, your rewards are automatically claimed and deposited to your wallet

### Service Fee

#### 📊 Fee Structure

To cover the gas costs of automating your transactions, a **10% fee** is applied to your weekly claimed rewards.

{% hint style="info" %}
The fee and cap are **community-driven**. Since we are still in the MVP phase, this setup helps us fine-tune the model and better understand the operational costs of keeping the service running. The community is welcome to propose adjustments and share their findings so we can collaboratively evaluate whether updates are appropriate.
{% endhint %}

**Fee Details:**

* **Rate**: 10% of your weekly rewards
* **Cap**: Maximum fee of **100 B3TR** per week
* **Benefit**: You don't pay any gas fees directly. The service handles all transactions for you

**Fee Example:**

| Your Weekly Rewards | Fee (10%)  | You Receive | Service Gas Coverage |
| ------------------- | ---------- | ----------- | -------------------- |
| 500 B3TR            | 50 B3TR    | 450 B3TR    | ✅ Full gas paid      |
| 1,500 B3TR          | 100 B3TR\* | 1,400 B3TR  | ✅ Full gas paid      |
| 5,000 B3TR          | 100 B3TR\* | 4,900 B3TR  | ✅ Full gas paid      |

{% hint style="info" %}
⚡ **Why the fee?** The service monitors all automation users and automatically processes transactions on your behalf. The 10% fee (capped at 100 B3TR) covers the gas costs and ensures reliable, hands-free automation every week.
{% endhint %}

### Important: Maintaining Eligibility

Your automation can be automatically **disabled** if any of the following occurs:

#### ⚠️ Auto-Disable Scenarios

**1. App Ineligibility**

* If all the apps you've selected for auto-voting become ineligible (removed or suspended), your automation will be turned off
* **Action required**: Update your app selections to include eligible apps, then re-enable auto-voting

**2. VOT3 Balance**

* If your VOT3 balance drops below 1 token at the time of snapshot
* **Action required**: Ensure you maintain at least 1 VOT3 in your wallet

**3. Stops Performing Sustainable Actions**

* If your [cumulative participation score](https://docs.vebetterdao.org/vepassport/checks/proof-of-participation#cumulative-score) drops below the threshold
* **Action required**: Continue to perform sustainable actions to maintain your eligibility

**4. Bot Detection**

* If you are flagged as a bot by any app owner during the snapshot
* **Action required**: File an appeal and complete the KYC flow. Then you need to come back to the [allocations](https://governance.vebetterdao.org/allocations) page to setup your automation again.

> 💡 **Tip**: Regularly check your automation status and app selections to ensure uninterrupted automation.

### Constraints <a href="#important-constraints" id="important-constraints"></a>

#### Manual Actions While Auto-Voting Is Active <a href="#manual-actions-while-auto-voting-is-active" id="manual-actions-while-auto-voting-is-active"></a>

When auto-voting is **active**, you **cannot** vote or claim rewards manually. The service handles everything automatically.

**Manual Claiming After 5 Days (Exception):**

* If the service hasn't claimed your rewards within **5 days** after a round ends, you can claim them manually
* This 5-day window ensures the automation service has enough time to process all auto-claims
* Your rewards are always protected. You'll never lose them

> ⚠️ **Remember**: Once automation is active, sit back and let the service do its job. Manual claiming is only available as a backup after 5 days.

### Understanding Your Status <a href="#understanding-your-status" id="understanding-your-status"></a>

Your automation status will show one of three states the allocations page:

#### 🟡 Auto-Voting Is Enabled <a href="#f0-9f-9f-a1-starting-next-round" id="f0-9f-9f-a1-starting-next-round"></a>

{% hint style="info" %}
**Auto-voting is enabled. It takes effect from the next round.**
{% endhint %}

* **What it means**: You just enabled auto-voting, but automation hasn't started yet
* **What happens**:
  * Current round: You need to vote manually (if you haven't already)
  * Next round: Automation will automatically kick in
* **Why the delay**: Auto-voting activates at the beginning of the next round to ensure a clean start with the snapshot

#### 🟢 Auto-Voting Is Active <a href="#f0-9f-9f-a2-auto-voting-active" id="f0-9f-9f-a2-auto-voting-active"></a>

{% hint style="info" %}
**Auto-voting is active. Your vote and rewards will be handled automatically..**
{% endhint %}

* **What it means**: Auto-voting is fully active and working
* **What happens**: The service automatically votes and claims rewards for you every week
* **No action needed**: Sit back and relax. Everything is handled automatically

#### :red\_circle: Auto-Voting Is Disabled

{% hint style="info" %}
**Auto-voting is disabled. It takes effect from the next round.**
{% endhint %}

* **What it means**: Auto-voting is now disabled
* **Why**:
  * You just toggled-off the automation. It will be deactivated from the next round onwards
  * It's been toggled-off  because you failed to maintain [eligibility](https://docs.vebetterdao.org/vebetter/automation#eligibility-requirements).

> 💡 **Tip**: Visit the DAO regularly to ensure automation remains active.

***

**Need Help?** If you have questions about auto-voting or encounter any issues, please contact our support team.
