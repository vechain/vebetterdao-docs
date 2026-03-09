# X2Earn Apps

The ‘X’ in X-2-Earn refers to the mathematical concept of the unknown variable. ‘X’ can be applied to any kind of sustainability ecosystem with an earn mechanism. For example, ‘Plant-2-Earn', for an ecosystem that rewards tree planting. Sweat-2-Earn, for Apps that reward working out, and so on. B3TR tokens distributed to X-2-Earn Apps are, in part, intended as a source of incentivization for users. They may serve other purposes, such as encouraging builders, or for marketing and sponsorships etc. B3TR reward distribution plans are at the discretion of projects and the voting community who will approve or disapprove of a project’s reward proposals through voting.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## Join VeBetter

As of the beginning of November, VeBetter allows apps to join the ecosystem through the endorsement of VeChain node holders. This shift from admin-controlled app selection to vechain community-based endorsement reflects the DAO’s commitment to decentralization.

* **Background Check and Creator NFT Requirement**: XApp creators begin by filling out a form on the VeBetter platform to initiate the process. If they pass the screening, they are minted a Creator NFT, which verifies them for participation in the endorsement process.
* **On-Chain XApp Submission**: Once they have received the Creator NFT, XApp creators can submit their XApp on-chain for endorsement consideration. This submission officially registers the XApp on VeBetter platform, pending endorsements from VeChain node holders.
* **Connecting with Endorsers**: After receiving the Creator NFT, XApp creators gain access to VeBetter Discord server, where they can connect with VeChain node holders who may endorse their XApp. This community platform allows them to build relationships with potential endorsers and formally submit their XApp for consideration on VeBetter platform.
* **XApp Endorsement**: XApps must accumulate an endorsement score of 100 to become eligible for allocation rounds. Achieving this threshold enables an XApp to participate in weekly rounds to receive a share of the B3TR allocation pool.

## VeChain Node Scores

VeChain node holders are key decision-makers in the X2Earn endorsement process. Each type of VeChain node, represented by an NFT in the holder’s wallet, has an assigned endorsement score based on its level. These scores determine the impact each endorsement has on an XApp’s eligibility.

| VeChain Node Level | Endorsement Score |
| ------------------ | ----------------- |
| Strength           | 2                 |
| Thunder            | 13                |
| Mjolnir            | 50                |
| VeThorX            | 3                 |
| StrengthX          | 9                 |
| ThunderX           | 35                |
| MjolnirX           | 100               |

A wallet can only hold one of these VeChain Node NFTs at a time, and to obtain a node, users must maintain a specific amount of VET in their wallet (see [vechain nodes](https://docs.vechain.org/core-concepts/nodes) for more info). The cumulative endorsement scores from nodes determine an XApp’s eligibility to join VeBetter and participate in allocation rounds.

## Voting Eligibility and Endorsement Status

Once an XApp is endorsed, reaching a cumulative endorsement score of 100, it becomes eligible to receive funds in the allocation rounds. This eligibility can be adjusted by VeChain node holders based on the app's performance and compliance.

### **XApp Endorsement**

An XApp is officially considered endorsed when it accumulates an endorsement score of 100. This score is the sum of all VeChain node endorsements for the app. Upon reaching this threshold, the XApp automatically qualifies for the next XAllocation Voting round and will continue participating in future rounds as long as its score remains at or above 100.

* **Endorsement Limitations**: Each VeChain Node NFT can endorse only one XApp at a time. Similarly, each account can endorse only one XApp unless it leverages the NodeManagement contract to delegate multiple VeChain nodes.
* **Endorsement Cap**: Once an XApp reaches the endorsed status with a score of 100, it cannot receive additional endorsements from other nodes.

### **XApp Un-Endorsement and Grace Period**

Node holders have the authority to withdraw their endorsement if they no longer wish to support a specific XApp. If an XApp’s score falls below the 100-point threshold, it enters a grace period. During this time, the XApp has a limited opportunity to regain endorsements and restore its score to 100. If it fails to meet this requirement before the grace period ends, it will be removed from the XAllocation voting round.

* **Retention of Past Allocations**: Although the XApp is removed from future allocation rounds, it still retains any allocations received from previous rounds. The XApp may rejoin allocation rounds if it regains the necessary endorsement score of 100.

### **Integrity Checks**

To ensure the consistency and validity of endorsements, VeBetter conducts periodic checks on endorsing nodes.

## App ID

When your app is submitted to the contract an `appId` is generated by hashing the app name. This id cannot be updated in the future, even if you change the name of your app.

## Signaling

Authorized apps can report addresses they have banned, such as  looters, bots or scammers. This helps  VeBetter and other apps detect and defend against harmful activity across the ecosystem.

If an address is flagged by mistake, VeBetter or selected app admins can review the case and reset its signal counter.&#x20;

**Community Dashboard:** [https://vechain-energy.github.io/vebetterdao-signal-admin/](https://vechain-energy.github.io/vebetterdao-signal-admin/)\
Build by the community, this dashboard shows all signaled wallets.&#x20;

## Metadata <a href="#x-apps-metadata" id="x-apps-metadata"></a>

When an app is added the following information is saved on-chain:

* **name**: the name of the app, cannot change once it's added.
* **app ID**: is the hash of the name of the app (from the previous point) and it's used as a unique identifier across all the smart contracts; once the app is added and this ID is generated it cannot be updated.
* **receiver address**: this is the address where the app will receive the allocation funds, also addressed to as "Treasury address".
* **creation timestamp**: the timestamp when the app was added.
* **metadata URI**: the IPFS CID containing all the information of the app, which will be used by the frontend to display the app details.
* **moderators**: a list of addresses that can update the metadataURI of an app.
* **admin**: this address can add/remove moderators, change the receiver address, transfer ownership to another admin, and update the metadata URI of the app.

The metadata URI needs to follow the following standard:

{% tabs %}
{% tab title="JSON" %}
```javascript
{
   "name":"My sustainable app",
   "description":"Run and get rewarded",
   "external_url":"https://openseacreatures.io/3",
   "logo":"https://storage.googleapis.com/opensea-prod.appspot.com/puffs/3.png",
   "header_image":"https://storage.googleapis.com/opensea-prod.appspot.com/puffs/3.png",
   "screenshots":[
      "https://storage.googleapis.com/opensea-prod.appspot.com/puffs/3.png",
      "https://storage.googleapis.com/opensea-prod.appspot.com/puffs/3.png",
      "https://storage.googleapis.com/opensea-prod.appspot.com/puffs/3.png"
   ],
   "social_urls":[
      {
         "name":"YouTube",
         "url":"https://www.youtube.com/@DLoaw"
      },
      {
         "name":"Instagram",
         "url":"https://www.instagram.com/@DLoaw"
      }
   ],
   "app_urls":[
      {
         "code":"play_store",
         "url":"https://www.playstore.com/@DLoaw"
      },
      {
         "code":"apple_store",
         "url":"https://www.applestore.com/@DLoaw"
      },
      {
         "code":"web_app",
         "url":"https://www.myapp.com"
      }
   ],
   "tweets": [],
   "categories": ["nutrition", "education-learning"]
}
```
{% endtab %}
{% endtabs %}
