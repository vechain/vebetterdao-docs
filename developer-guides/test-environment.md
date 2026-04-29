# Test Environment

You have two testing environments: testnet and an instance of the blockchain running locally (a [solo node](https://docs.vechain.org/core-concepts/networks/thor-solo-node)).&#x20;

Testing your app on testnet is more straightforward since all contracts are already deployed by us and you can use our testnet dApp to add apps, interact with smart contracts, and manage reward distribution.

Testing on solo will be a bit more complicated because you will need to deploy some mocked VeBetter contracts by yourself, but we tried to make that easy as well.

## Testnet

Use our testnet environment, running on vechain testnet network, to create apps, interact with smart contracts, and manage reward distribution.

* Go to [https://staging.testnet.governance.vebetterdao.org/apps](https://staging.testnet.governance.vebetterdao.org/apps)
* Create a new app following the form (it will mint for you a creator nft)
* After adding the app it needs to pass the endorsement check, navigate to [https://staging.testnet.governance.vebetterdao.org/admin](https://staging.testnet.governance.vebetterdao.org/admin), select the "X2Earn Apps" tab, and click the "Check Endorsement" button for your app.
* Then you can navigate to [https://staging.testnet.governance.vebetterdao.org/admin](https://staging.testnet.governance.vebetterdao.org/admin), start a new round by clicking "Start new round & claim allocations" (you will probably need to do it a couple of times before you begin getting b3tr from allocations)
* To allow your contract to distribute the rewards, you need to click the cogs icon, enter the Settings section of your app, and add the address of your contract (which will call the `distributeReward` function) as a "Reward Distributor".

That's it, you are all set and can distribute rewards through your backend or smart contract.

Testnet contract addresses:

```json
"B3TR": "0x95761346d18244bb91664181bf91193376197088",
"B3TRGovernor": "0xc30b4d0837f7e3706749655d8bde0c0f265dd81b",
"Emissions": "0x66898f98409db20ed6a1bf0021334b7897eb0688",
"GalaxyMember": "0x38a59fa7fd7039884465a0ff285b8c4b6fe394ca",
"TimeLock": "0x835509222aa67c333a1cbf29bd341e014aba86c9",
"Treasury": "0x3d531a80c05099c71b02585031f86a2988e0caca",
"VOT3": "0x6e8b4a88d37897fc11f6ba12c805695f1c41f40e",
"VoterRewards": "0x851ef91801899a4e7e4a3174a9300b3e20c957e8",
"X2EarnApps": "0x0b54a094b877a25bdc95b4431eaa1e2206b1ddfe",
"X2EarnRewardsPool": "0x2d2a2207c68a46fc79325d7718e639d1047b0d8b",
"XAllocationPool": "0x6f7b4bc19b4dc99005b473b9c45ce2815bbe7533",
"XAllocationVoting": "0x8800592c463f0b21ae08732559ee8e146db1d7b2",
"NavigatorRegistry": "0x15a38b65f26bdbca50addf3865732613a45bbc00"
```

## Local node

Testing on the solo node (aka: local node running on your computer simulating VeChain blockchain) is a bit more complicated than testing on testnet, since you will need to deploy the VeBetter contracts locally and interact with them to create your app, obtain the APP\_ID, start an allocation round, vote, start round again, and add the reward distributor.

We recommend following the testnet path, but if you need to do it on the solo network you will need to mock a few VeBetter contracts:

* `B3TR` token mock: you need a fake token to distribute
* `X2EarnApps` mock: you need a contract where to add your app, generate an APP\_ID and add a reward distributor
* `X2EarnRewardsPool` mock: you need a contract to distribute the rewards

While the `X2EarnRewardsPoolMock` contract can be the exact same contract deployed on mainnet and testnet, the other two can be customized in order to have only the essential logic needed for running tests.

You can take a look at the mocked contracts in the [X-App-Template](https://github.com/vechain/x-app-template) repository, under the `contracts/mocks` folder.

After you added the mock contracts to your repository you should adjust the deploy script in a way that you will deploy and configure the mocked contracts only if you are deploying to the `vechain_solo` network.

Your script should look something like this:



```typescript
import { ethers, network } from "hardhat";
import { Challenges, Cleanify, RolesManager } from "../../typechain-types";
import { networkConfig } from "@repo/config";
import { deployProxy } from "../helpers/upgrades";
import { faker } from "@faker-js/faker";

let REWARD_TOKEN_ADDRESS = "0xE5FEfcB230364ef7f9B5B0df6DA81B227726612b"; // mainnet address

export async function deploy(): Promise<{
    mySustainableContractAddress: string;
}> {
    console.log(
        `Deploying on ${network.name} (${networkConfig.network.defaultNodeUrl})...`
    );

    console.log(`Deploying my app contract...`);
    const MySustainableContract = await ethers.getContractFactory("MySustainableContract");
    const mySustainableContract = await MySustainableContract.deploy();
    await mySustainableContract.waitForDeployment();
    console.log(`Sustainable Contract deployed to ${await mySustainableContract.getAddress()}`);

    if (network.name === "vechain_solo") {
        console.log(`Deploying mock RewardToken...`);
        const RewardToken = await ethers.getContractFactory("B3TR_Mock");
        const rewardToken = await RewardToken.deploy();
        await rewardToken.waitForDeployment();
        REWARD_TOKEN_ADDRESS = await rewardToken.getAddress();
        console.log(`RewardToken deployed to ${REWARD_TOKEN_ADDRESS}`);

        console.log("Deploying X2EarnApps mock contract...");
        const X2EarnApps = await ethers.getContractFactory("X2EarnAppsMock");
        const x2EarnApps = await X2EarnApps.deploy();
        await x2EarnApps.waitForDeployment();
        console.log(`X2EarnApps deployed to ${await x2EarnApps.getAddress()}`);

        console.log("Deploying X2EarnRewardsPool mock contract...");
        const X2EarnRewardsPool = await ethers.getContractFactory(
            "X2EarnRewardsPoolMock"
        );
        const x2EarnRewardsPool = await X2EarnRewardsPool.deploy(
            deployer.address,
            REWARD_TOKEN_ADDRESS,
            await x2EarnApps.getAddress()
        );
        await x2EarnRewardsPool.waitForDeployment();
        console.log(
            `X2EarnRewardsPool deployed to ${await x2EarnRewardsPool.getAddress()}`
        );

        console.log("Adding app in X2EarnApps...");
        await x2EarnApps.addApp(deployer.address, deployer.address, "MySustainableApp");
        const appID = await x2EarnApps.hashAppName("MySustainableApp");

        console.log(`Funding contract...`);
        await rewardToken.approve(
            await x2EarnRewardsPool.getAddress(),
            ethers.parseEther("10000")
        );
        await x2EarnRewardsPool.deposit(ethers.parseEther("2000"), appID);
        console.log("Funded");

        console.log("Add MySustainableContract as distributor...");
        await x2EarnApps.addRewardDistributor(
            appID,
            await mySustainableContract.getAddress()
        );
        console.log("Added");

        console.log(
            "Set APP_ID and X2EarnRewardsPool address in Challenges contract..."
        );
        await mySustainableContract.setVBDAppId(appID);
        await mySustainableContract.setX2EarnRewardsPool(
            await x2EarnRewardsPool.getAddress()
        );
        console.log("Set");
    }
    
    return {
        mySustainableContract: await mySustainableContract.getAddress()
    };
}
```

That's it, you can now deploy your contracts locally/run tests where you simulate the VeBetter contracts, your app added to the ecosystem, and tokens distribution.
