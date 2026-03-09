# Reward Metadata

The Rewards Metadata feature allows applications to enrich reward distributions with additional contextual information. This is facilitated through the `distributeRewardWithProofAndMetadata` function, which accepts a JSON-formatted string as metadata. \
\
The provided metadata is then emitted via the `RewardMetadata` event, enabling off-chain services to efficiently index, analyze, and utilize the data for tracking and reporting purposes.

### Suggested Metadata Structure

To maintain consistency across applications, it's recommended to follow a standardized metadata structure. Below is an example of a JSON-formatted metadata string:

```json
{
  "location": {
    "city": "Berlin",
    "country": "Germany",
    "region": "EU"
  },
  "referral_source": "social_media",
  "campaign_id": "earth_day_2025"
}

```

{% hint style="warning" %}
Keep in mind that including location metadata may require updating your privacy policy and terms of service.
{% endhint %}

### Examples

To provide metadata you need to distribute the rewards through the X2EarnRewardsPool contract with the following function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import { ProviderInternalHDWallet, ThorClient, VeChainProvider, VeChainSigner } from "@vechain/sdk-network";
import { X2EarnRewardsPool } from "@vechain/vebetterdao-contracts";

const thor = ThorClient.fromUrl("https://mainnet.vechain.org");
const provider = new VeChainProvider(
  thor,
  new ProviderInternalHDWallet("your space separated mnemonic".split(" "))
);
const rootSigner = await provider.getSigner();

// Define metadata as an object
const metadata = {
  "location": {
    "city": "Berlin",
    "country": "Germany",
    "region": "EU"
  },
  "referral_source": "social_media",
  "campaign_id": "earth_day_2025"
};

// Call the method
const x2EarnRewardsPoolContract = thor.contracts.load(
  X2EarnRewardsPool.address.mainnet,
  X2EarnRewardsPool.abi,
  rootSigner as VeChainSigner
);

const tx = await x2EarnRewardsPoolContract.transact.distributeRewardWithProofAndMetadata(
  APP_ID,
  amount,
  receiverAddress,
  ["link", "image"],
  ["https://link-to-proof.com", "https://link-to-image.com/1.png"],
  ["waste_mass"],
  [100],
  "User performed a sustainable action on my app",
  JSON.stringify(metadata) // Convert metadata object to JSON string
);

await tx.wait();

```
{% endtab %}

{% tab title="Solidity" %}
```python
import "./interfaces/IX2EarnRewardsPool.sol";

contract MyContract {
    function sendRewardWithMetadata() onlyAdmin {
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
        
        // Constructing metadata JSON string
        string memory metadata = '{"location":{"city":"Berlin","country":"Germany","region":"EU"},"referral_source":"social_media","campaign_id":"earth_day_2025"}';
    
        
        // If distributeReward fails, it will revert
        x2EarnRewardsPool.distributeRewardWithProofAndMetadata(
            VBD_APP_ID,
            rewardAmount,
            receiver,
            proofTypes,
            proofUrls,
            impactTypes,
            impactValues,
            "User participated in a solo cleanup",
            metadata
        );
    }
}

```
{% endtab %}
{% endtabs %}

### Emitted Event: `RewardMetadata`

Upon successful execution of the `distributeRewardWithProofAndMetadata` function, the `RewardMetadata` event is emitted with the following parameters:

* **amount** (`uint256`): The distributed reward amount.
* **appId** (`bytes32`): The application identifier.
* **receiver** (`address`): The address receiving the reward.
* **metadata** (`string`): The JSON-formatted metadata string.
* **distributor** (`address`): The address initiating the distribution.

This event allows off-chain systems to listen for and process reward distributions along with their associated metadata.

### Guidelines

* **Optional Usage**: The `metadata` is optional. If your application doesn't require additional context, you can continue using the existing `distributeRewardWithProof` function.
* **Standardization**: Adhere to the suggested metadata structure to ensure consistency and facilitate seamless data integration across the ecosystem.
* **Data Validation**: Implement validation checks to ensure the metadata JSON is correctly formatted and contains relevant information before invoking the distribution function.
