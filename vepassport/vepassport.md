# VePassport

**VePassport** is a system that allows Apps on VeChain to determine whether a wallet belongs to a bot or a real person. The Apps using this technology include, first and foremost, **VeBetter**, which verifies the authenticity of wallets during voting. Other Apps within the VeBetter ecosystem, often facing issues with fake accounts and Sybil attacks, can also access this information.

This decentralized identification framework enables a secure, Sybil-resistant environment that drives participation and promotes a more transparent, sustainable ecosystem.

{% hint style="info" %}
Contracts can be found at the following repository: [https://github.com/vechain/vebetterdao-contracts ](https://github.com/vechain/vebetterdao-contracts)\
\
`Mainnet Address: 0x35a267671d8EDD607B2056A9a13E7ba7CF53c8b3`
{% endhint %}

## Proof of Personhood

There are several modules that VePassport uses to determine if a wallet is a bot or if it belongs to a real user:&#x20;

* Proof of Participation in the VeBetter Ecosystem
* Proof of Investment
* Proof of Identity
* Whitelisting and Blacklisting
* Bot Signaling

<figure><img src="../.gitbook/assets/VePassport long term vision (2).png" alt=""><figcaption><p>Long term VePassport</p></figcaption></figure>

In the current implementation to determine if a wallet is legitimate, several factors are considered: whether it is on the whitelist or blacklisted, and the number of actions performed within a specific time range, while Proof of Investment and Identity will be integrated with the next releases.

<figure><img src="../.gitbook/assets/VePassport long term vision (3).png" alt=""><figcaption><p>Initial implementation</p></figcaption></figure>

Apps can interact with VePassport to check if a user is a person (now or in a specific block number) or can call each module separately to only get the participation score, or if the user is KYCed, or if it's whitelisted, blackilsted or signaled as a bot by other apps.

VePassport is designed to be expanded in the future with new modules.

## Proof of Participation

Users need to complete 3 Better actions within a 12-week period to be considered a person. This requirement encourages greater participation in the ecosystem and ensures users are active contributors to the DAO.

An action is defined as a reward that you receive from an app  (e.g., receiving a reward for drinking coffee in a sustainable cup with Mugshot).

Every time a user engages with an app and receives a reward their passport accumulates points. The amount of points is determined by the security level of the app he interacted with. An app with a **low security** score is worth 100 points, **medium** is 200, **high** is 400, and **none** is 0.

## Proof of Investment

Another way to verify a wallet’s legitimacy is through the **GM NFT level** that the wallet holds.\
If the address holds a GM NFT with a level higher than 1 then it indicates that the owner has made a significant investment, since upgrading levels on that NFT is not free, suggesting that the wallet is more likely to be genuine.&#x20;

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>GM level 1</p></figcaption></figure>

## Whitelisting, Blacklisting, and Bot Signalling

X2earn apps can also flag a wallet as a bot or suspicious user. If a wallet receives enough signals to exceed a certain threshold, it will not be considered a real person.&#x20;

Additionally, some authorized entities can **blacklist** or **whitelist** a wallet. Blacklisting might occur if a wallet is found to be part of a network of fake accounts. If a wallet is mistakenly blacklisted, it can request to be reinstated.&#x20;

Similarly, a wallet can be added to the whitelist, especially if the user has completed a **KYC** (Know Your Customer) process, thereby ensuring that the account is legitimate.

## Proof of Identity

Various levels of KYC (Know Your Customer) can be applied in the future, ranging from linking social profiles to full identity verification. This feature will remain optional, and will instantly provide layers of security and trust for user identities.

{% hint style="danger" %}
This proof is not implemented yet.
{% endhint %}

## Delegation

To better increase the interoperability across the VeBetter and VeChain ecosystem, VePassport offers the possibility to delegate your passport to other addresses.

You can even customize your passport by attaching to it other accounts you own.
