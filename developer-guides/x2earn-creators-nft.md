# X2Earn Creators NFT

The **X2Earn Creator NFTs** are integral to VeBetter’s process for managing and validating new XApp submissions. These NFTs serve as an access key for creators, enabling them to submit their applications on-chain and engage with the VeBetter endorsement process through VeChain node holders.

## **Overview**

* **Purpose**: X2Earn Creator NFTs are used within VeBetter to authorize new XApp submissions and provide access to the endorsers (VeChain node holders). These NFTs ensure that only vetted and verified creators can submit apps for consideration.
* **Standard**: Creator NFTs are non-transferable tokens compliant with the ERC721 standard, implemented using the OpenZeppelin (OZ) library.

## **Minting and Eligibility**

* **Minting Authority**: Only the X2Earn App Review Panel has the authority to mint Creator NFTs. The panel grants these NFTs if they deem the app suitable, legitimate, and ready for the endorsement process within VeBetter.
* **Non-Transferability**: Creator NFTs are non-transferable, meaning they cannot be sold or transferred to another user, ensuring that only the verified creator retains access.

## **Functionality and Permissions**

* **Submission Access**: Once an app creator have granted a Creator NFT, they can submit their XApp on-chain for endorsement. This submission formally registers the app within the VeBetter ecosystem, allowing it to seek endorsements from node holders.
* **Additional NFT for Team member**: After the initial submission, the XApp admin (creator) can add up to two additional creators, for a maximum of three creators per application. Each additional creator will receive their own Creator NFT.
* **Single NFT Limitation**: Users are limited to holding only one Creator NFT at any given time. If a user wishes to contribute to another application, they must either revoke their current Creator NFT from the xapp admin or use a separate account.&#x20;

## **Blacklisting and Burn Mechanism**

* **Blacklisting Policy**: If an XApp is blacklisted for any reason, and the creator is not associated with any other active XApp, their Creator NFT will be burned. This prevents creators of non-compliant or blacklisted apps from submitting new projects without re-approval.
* **NFT Burn**: The burning mechanism ensures that blacklisted creators cannot bypass VeBetter’s review process by holding onto their Creator NFT. This policy enhances the integrity of the ecosystem by holding creators accountable for their submissions.

## Key Points for Developers

* **Token Standard**: ERC721 (non-transferable)
* **Minting Authority**: X2Earn App Review Panel
* **Max NFTs per User**: One NFT per user, regardless of the number of XApps
* **Additional NFTs**: Admin have the ability to mint up to two extra NFTs for team members. These additional creators will be restricted to their assigned app only and cannot use their creator NFTs to submit new apps.
* **Single Creator Association**: Each creator is assigned to exactly one app.
