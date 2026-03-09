# Security Considerations

X2Earn Apps should consider implementing necessary security measures to guard against farmers unfairly obtaining b3tr tokens.&#x20;

### Farming Approaches

* _Single Person, Single Wallet_ -> In this scenario the attacker is a single person using a single wallet address. They try to repeat the same claim for b3tr rewards multiple times for the same wallet address. If the dApp is using AI for image validation, they could attempt to use a fake image over and over again.
* _Single Person, Multiple Wallets_ -> In this scenario the attacker is a single person but now using multiple wallet addresses. For example they might submit a claim for a reward, then logout of the dApp then login with a different wallet address in VeWorld and repeat the same claim. The attacker then repeats this exercise switching between different accounts to make multiple claims for rewards.
* _Scripts (aka Bots)_ -> This is where the attacker has coded a script that submits a claim for rewards. The script might try a single wallet or use multiple wallets to submit the claim for a reward. If the dApp is using AI for image verification, the script might generate a new image or adjust a previously created image.
* _Multiple Collaborating People, Multiple Wallets_ -> In this scenario a group of attackers are collaborating together, they may share the same wallet, share social login details. As they are collaborating together they are collecting rewards for the group, which they can share between the group later.
* _Exploiting Vulnerabilities_ -> In this scenario the attacker exploits vulnerabilities in smart contracts or back-end API services to create Bots or manually bypass the front-end of the dApp.

### Potential Solutions

#### Securing Back-End APIs

* _Certificate based authentication_ -> For example if there is a `/account` endpoint this should be secured with the signed certificate the user signs in VeWorld. The back-end can then validate the certificate, and use the signed address in from the certificate to identify the users wallet.
* _Captcha verification_ -> For example if there is a `/claim` endpoint this should be secured with a ReCaptcha. This is to protect against scripted/bot attacks, as it ensures that the back-end APIs can only be used from the front-end of the dApp.
* _CORS domains_ -> This is to ensure that the back-end API's can only be called with a request from the same domain. For example if the API is running at `api.myapp.com` and a request to it is made from `api.fakeapp.com`, it will be rejected.
* _Rate limiting_ -> Depending on the design of the dApp a user will only be allowed to make a limited number of claims for rewards per day, per week. There are different strategies here, for example:
  * Inspecting the date-time when the users address last received a reward, and not allowing more claims for a time period after that. For example a user can't make another claim until 24hrs after their previously paid reward. This can be implemented in the back-end database or by reading the last reward event on-chain.
  * Determining the current round (week) and how many claims the user has submitted in that timeframe. Thus limiting the user to a fixed number of claims per week.
  * Using device identification techniques to uniquely identify the mobile device the user is using, and limiting the number of claims for rewards not just on address but also on device.

#### How Sustainable Actions are Verified

* _AI based validation_ -> The prompts used to validate an image should be thoroughly tested to ensure that the AI is validating as expected. If the AI is used to extract data out of an image (e.g. A receipt) it should be tested to ensure that _hallucinations_ are not occurring. The AI should be asked to produce a confidence score, if that is below a threshold the image could be flagged for manual verification.
* _No unique identifier_ -> To verify a sustainable action there should be some indisputable evidence of it. For example a social media post, data from an external system, a receipt as proof of purchase or a date-time stamp. In the case of AI image verification, the AI should be prompted to recognise relevant data in the image that can be used for a uniqueness check.&#x20;

#### Suspicious Behaviour & Ban List

Apps should have the means to identify suspicious behaviour within their dApp, for example rewards being paid every 10 seconds to accounts that have no other transaction history. Apps should have the means to ban accounts or entire devices from using their service.

#### Management of Private Keys

The distributor account private key should be secure within the dApp and not be able to be sniffed in the traffic the dApp generates. If the dApp is using back-end contracts or APIs to sign the reward transaction these should have a secure way of storing the private keys.

#### Device Fingerprinting

Farmers might attempt to install VeWorld (or your native app), multiple times on the same device, and use a different wallet per instance of the app. This is especially true of Android devices, where software exists to allow multiple instances of an app to be installed. A dApp should consider using tools such as FingerprintJS (or its paid version), to identify a users device, and ban the device rather than just the users account(s). If developing a native dApp, protections against multiple installs should be built into the dApp.



### Other Strategies

* _Management of funds_ -> The allocation the dApp receives from the DAO on a weekly basis is _at risk_ if there are vulnerabilities within the dApp. The dApp owner if concerned about farmers should reduce the b3tr balance of the dApp by withdrawing funds to their treasury account and drip feeding it back to the dapp when needed. Once the concerns are alleviated this strategy can end. The dApp owner can also mitigate this risk by setting a portion of the weekly allocation for distribution and another portion as treasury funds. Treasury funds can be withdrawn at any time.
* _Identify verification_ -> Apps can also offer login methods other to VeWorld, for example Google, Facebook, Twitter. In these cases dApp owners should consider validation of the users social media profiles, for example does the facebook profile have a complete profile, was the google account created in last 30mins. Apps could also consider sending verification emails.
* &#x20;_Progressive unlocking_ -> A new account in the dApp might have tigher rate limits and lower rewards. As the user continues to use the dApp they progressively unlock higher rate limits and higher rewards. This means an attacker has to spend more efforts to reach the higher rewards slowing their progress.
* _Scaling Rewards by Demand ->_ This is a strategy to scale down user rewards in peek demands and scale them back up at quiet times. The goal is to make the weekly allocation a dApp received last the full week, but also guard against spikes in users (or bots) claiming rewards. One way to do this is to compute the _average rewards per second_ since the start of the week, and project it forward to the end of the week, and see if above/below the Apps weekly budget. If above budget, the dApp can then scale down users rewards until back on track.

### Conclusion

Stopping farmers is a difficult task and in some scenarios very difficult to achieve without impacting genuine users. Often the approach is to slow the farmers progress so that their effort/reward ratio is no longer worth it.

### Recommended Service: _Guardian_ (Advanced Fraud Detection)

To help Apps tackle many of the challenges described above, we recommend evaluating a service called [**Guardian**](https://docs.guardianstack.ai/) — a modern, enterprise-grade fraud detection and risk assessment platform.&#x20;

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

Guardian was built from real-world experience fighting the exact pain points outlined in this document: bot activity, multi-wallet farming, VPN/proxy abuse, device spoofing, browser tampering, and coordinated fraud attempts. It provides accurate detection signals and behavioural risk scoring that can be integrated directly into front-end or back-end flows, making it especially valuable for ecosystems like **VeBetter**, where protecting reward integrity is critical.&#x20;

This is **not** a paid advertisement; Guardian is simply a tool that aligns well with the needs of sustainability-focused applications.&#x20;

For projects interested in trying it, a **50% discount** is available using the code **VEBETTER50** at checkout.

Guardian Website: [https://dashboard.guardianstack.ai/](https://dashboard.guardianstack.ai/)

Guardian Docs: [https://docs.guardianstack.ai/](https://docs.guardianstack.ai/)
