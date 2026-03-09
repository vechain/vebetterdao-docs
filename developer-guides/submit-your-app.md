# Submit Your App

As of the start of November the VeChain node holders, referred to as "endorsers," are serving as the primary decision-makers in determining which XApps are admitted to the VeBetter ecosystem. Developers and innovators can engage with the ecosystem and submit their [X2Earn applications](../vebetter/x2earn-apps.md) through a structured process that includes community endorsement and voting.

## Submission Process

To submit an app to the VeBetter ecosystem, every project must first acquire a Creator NFT, which verifies the creator and allows them to participate in the endorsement process. Here’s how it works:

1. **Acquiring the Creator NFT**
   * **Application Process**: XApp creators begin by filling out a form on the VeBetter platform, which initiates a background check to verify the team and the app’s legitimacy.
   * **Alternative Paths**: Creators can also apply for a grant through the [VeChain Grants program](https://vechain.org/grants/). If successful, they will receive funding as well as a Creator NFT, which validates their project within the ecosystem.
2. **On-Chain XApp Submission**\
   After receiving the Creator NFT, XApp creators can submit their app on-chain for [endorsement](../vechain-node-and-staking-guides/legacy-vechain-node-guides-pre-stargate/endorsement-guide-for-vechain-node-holders.md) consideration. This official submission registers the app on the VeBetter platform, allowing it to seek endorsements from VeChain node holders.
3. **Connecting with Endorsers**\
   Upon acquiring the Creator NFT, XApp creators gain access to the VeBetter XApps Creators Discord server, where they can network with VeChain node holders who may endorse their app. This community platform provides opportunities for creators to present their app, build relationships with potential endorsers, and gain the necessary endorsement score to enter allocation rounds.
4. **Endorsement and Funding Eligibility**\
   Once an app is submitted and endorsed with a cumulative score of 100,  it will officially be added to the VeBetter ecosystem. It will be eligible to be weekly funded with B3TR tokens as a result of the [allocation rounds](../vebetter/x2earn-allocations.md): you will compete against other apps to receive as many B3TR tokens as possible, with holders of the token that will vote for the app they think deserves it the most.

{% content-ref url="../vechain-node-and-staking-guides/legacy-vechain-node-guides-pre-stargate/endorsement-guide-for-vechain-node-holders.md" %}
[endorsement-guide-for-vechain-node-holders.md](../vechain-node-and-staking-guides/legacy-vechain-node-guides-pre-stargate/endorsement-guide-for-vechain-node-holders.md)
{% endcontent-ref %}

## **On-Chain App Submission Requirements**

When submitting your app to VeBetter you will need to provide 2 important information:

* **Treasury address**: this is the address where B3TR tokens will be sent in case you will need to withdraw some tokens from the received weekly allocations (eg: you need some B3TR for marketing, team shares, etc.). This address can be a multi-sig or a simple EOA, it's up to you.
* **Admin address**: this is the address that has superpowers for your app; it can update any details of the app, change the Treasury address, or move the ownership to another wallet. You can use the same wallet you use for the Treasury, another multi-sig, or a simple EOA as well.

## Your APP ID

Once your app is added **you will obtain an APP\_ID** that VeBetter and other projects will use to recognize your app.\
You **will also need to use your Admin address to add a&#x20;**_**Reward Distributor**_ to distribute your rewards to the users. This is a simple address and it should belong to the contract or wallet that you will use to distribute the rewards. Be aware that this address can also withdraw funds, so protect its access correctly.

## B3TR Allocations

You will **receive your first B3TR tokens at least one week after joining VeBetter**, following the completion of at least one voting round. Therefore, you will need to address the scenario where people might see your app in VeBetter and attempt to use it before you are able to distribute rewards.

## Your App categories

When submitting your app, you can select up to 2 categories that best describe its purpose and functionality. These categories help VeBetter filter and organize apps effectively, ensuring users can easily discover relevant app.

Categories in your App Metadata are stored using their `id` value, while the `name` is simply how the category will appear inside VeBetterDAO during the submission  or the metadata modification process.&#x20;

```typescript
[
  { "id": "others",                        "name": "Others" },
  { "id": "education-learning",            "name": "Learning" },
  { "id": "fitness-wellness",              "name": "Lifestyle" },
  { "id": "green-finance-defi",            "name": "Web3" },
  { "id": "green-mobility-travel",         "name": "Transportation" },
  { "id": "nutrition",                     "name": "Food & Drinks" },
  { "id": "plastic-waste-recycling",       "name": "Recycling" },
  { "id": "renewable-energy-efficiency",   "name": "Energy" },
  { "id": "sustainable-shopping",          "name": "Shopping" },
  { "id": "pets",                          "name": "Pets" }
]

```



{% hint style="info" %}
If you are submitting a **native application,** please link to your app a landing page.&#x20;
{% endhint %}
