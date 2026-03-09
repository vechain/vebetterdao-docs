# Sustainability Proof and Impacts

The proof will be stored on-chain in the form of an emitted event and will have the following JSON format:

```json
{
   version: 2,
   description: 'The description of the action',
   proof: { 
      image: 'https://image.png', 
      link: 'https://twitter.com/tweet/1' 
   },
   impact: { 
      carbon: 100, 
      water: 200 
   }
}
```

## Parameters

You will provide this proof as a parameter when calling the distribute rewards function in the `X2EarnRewardsPool` contract.

* **proofTypes**: an array with the proof types that you are providing. Eg:\
  `["link", "image"]`
* **proofValues**:  an array with the values of the types provided in the previous array. Eg: `["https://twitter.com/tweet/1233121231", "https://whatever.com/image.png"]`
* **impactCodes**: an array with the codes of impacts you are providing. Eg:\
  `["water", "timber"]`
* **impactValues**: an array with the values of the impacts provided in the previous array. Eg:\
  `[1000, 23]`
* **description**: you can attach also a description to your action, it's optional.

When calling this function it is mandatory to provide at least proof or impact, if none of them is provided then the transaction will revert.

The transaction will revert also if something is wrong with the data you pass.

## Examples

To provide proofs and impacts you need to distribute the rewards through the X2EarnRewardsPool contract with the following function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import { ProviderInternalHDWallet, ThorClient, VeChainProvider, VeChainSigner } from "@vechain/sdk-network"
import { X2EarnRewardsPool } from "@vechain/vebetterdao-contracts"

const thor = ThorClient.fromUrl("https://mainnet.vechain.org")
const provider = new VeChainProvider(
  thor,
  new ProviderInternalHDWallet("your space separated mnemonic".split(" ")),
)
const rootSigner = await provider.getSigner()

// Call the method
const x2EarnRewardsPoolContract = thor.contracts.load(X2EarnRewardsPool.address.mainnet, X2EarnRewardsPool.abi, rootSigner as VeChainSigner)
const tx = await x2EarnRewardsPoolContract.transact.distributeRewardWithProof(
  APP_ID,
  amount,
  receiverAddress,
  ["link", "image"],
  ["https://link-to-proof.com", "https://link-to-image.com/1.png"],
  ["waste_mass"],
  [100],
  "User performed a sustainable action on my app",
)

await tx.wait()
```
{% endtab %}

{% tab title="Solidity" %}
```solidity
import "./interfaces/IX2EarnRewardsPool.sol";

contract MyContract {
    function sendReward() onlyAdmin {
        // this declaration is needed because solidity has 
        // an issue with dynamic vs fixed-size arrays
        string[] memory proofTypes = new string[](1);
        proofTypes[0] = "link";
        
        string[] memory proofUrls = new string[](1);
        proofUrls[0] = action.proofUrl;
        
        string[] memory impactTypes = new string[](1);
        impactTypes[0] = "waste_mass";
            
        uint256[] memory impactValues = new uint256[](1);
        // let's pretend you have another function that calculates your impact
        impactValues[0] = calculateWasteMass(
            challenge.litterSize,
            challenge.litterCount
        );
        
        IX2EarnRewardsPool x2EarnRewardsPool = IX2EarnRewardsPool(x2EarnRewardsPoolAddress);
        
        // If distributeReward fails, it will revert
        x2EarnRewardsPool.distributeRewardWithProof(
            VBD_APP_ID,
            rewardAmount,
            receiver,
            proofTypes,
            proofUrls,
            impactTypes,
            impactValues,
            "User participated in a solo cleanup"
        );
    }
}

```
{% endtab %}
{% endtabs %}

## Proofs <a href="#title-text" id="title-text"></a>

You can attach proof of the user's sustainable action when you distribute the reward. The proof can be a link, video, photo, and text. You can provide more than one proof (eg: a photo and a link).

The current proof types supported are:

<pre><code><strong>"image"
</strong><strong>"link"
</strong><strong>"text"
</strong><strong>"video"
</strong></code></pre>

## Impacts <a href="#title-text" id="title-text"></a>

Determining the environmental impact of each sustainable action will require the development of a set of criteria and metrics for each type of action.

{% hint style="warning" %}
**Warning: the impact values MUST be a number and the unit is considered the default minimum (so milliliters, grams, wh, etc.). So if you saved 1 litter of water then you should put it as an impact of `"water" : "1000"`.**
{% endhint %}

### Categories <a href="#categories" id="categories"></a>

1. **Carbon Footprint Reduction**\
   **Key:** `carbon`\
   **Description:** Measures the decrease in greenhouse gas emissions.\
   **Metric:** Grams (g) of CO2 equivalent reduced or removed.
2. **Resource Conservation**\
   **Key:** `water` (for water conservation), `energy` (for electricity conservation)\
   **Description:** Assesses the reduction in the use of natural resources like water, electricity, and raw materials.\
   **Metric:** Milliliters (ml) of water saved, watt-hours (Wh) of electricity saved.
3. **Waste Reduction**\
   **Key:** `waste_mass`\
   **Description:** Evaluates the decrease in waste generated and improves waste management.\
   **Metric:** Grams (g) of waste diverted from landfills, number of items recycled.
4. **Timber Conservation**\
   **Key:** `timber`\
   **Description:** Measures the reduction in the use or waste of timber resources.\
   **Metric:** Grams (g) of timber saved.
5. **Plastic Reduction**\
   **Key:** `plastic`\
   **Description:** Evaluates the decrease in the use or waste of plastic materials.\
   **Metric:** Grams (g) of plastic saved or reduced.
6. **Education Time**\
   **Key:** `education_time`\
   **Description:** Measures the amount of time a user spends learning about a sustainability topic.\
   **Metric:** Seconds spent learning.
7. **Trees Planted**\
   **Key:** `trees_planted`\
   **Description:** Tracks the number of trees planted as part of sustainability efforts.\
   **Metric:** Number of trees planted.
8. **Calories Burned**\
   **Key:** `calories_burned`\
   **Description:** Measures the energy expenditure from physical activity or exercise, promoting health and sustainability.\
   **Metric:** Calories (kcal) burned.
9. **Sleep Quality**\
   **Key:** `sleep_quality_percentage`\
   **Description:** Assesses the quality of sleep as a percentage, encouraging better health and productivity.\
   **Metric:** Percentage (%) of sleep quality improvement.
10. **Clean Energy Production**\
    **Key:** `clean_energy_production_wh`\
    **Description:** Measures the amount of clean energy produced, contributing to sustainable energy solutions.\
    **Metric:** Watt-hours (Wh) of clean energy generated.

The following are the impact codes you will need to use:

```
"carbon"
"water"
"energy"
"waste_mass"
"education_time",
"timber"
"plastic"
"trees_planted"
"calories_burned"
"sleep_quality_percentage"
"clean_energy_production_wh"
```

#### We ask each app to find a way to calculate the impact they are having based on the previously listed categories. If your app does not fit in any of the above categories please feel free to submit your own category and impact definition. <a href="#action-specific-impact-models" id="action-specific-impact-models"></a>

## **Deprecated template (version 1)**

**If no "version" field is found in the the json proof then you should consider it as version 1.**

```json
{
  "app_name": "cleanify",
  "action_type": "litter_picking",
  "proof": {
    "proof_type": "link",
    "proof_data": "https://x.com/TengMab95659/status/1814152547202195788"
  },
  "metadata": {
    "description": "@TengMab95659 picked up 8 small pieces of litter.",
    "additional_info": ""
  },
  "impact": {
    "waste_mass": "300",
    "biodiversity": "1"
  },
}
```

Deprecated impacts:

```
waste_items
people
biodiversity
```

{% hint style="success" %}
If you want to store additional data beyond impacts or proofs, you might consider using the reward metadata function, which allows you to include other meaningful extra information. [Go to Reward Metadata Docs](reward-metadata.md)
{% endhint %}

