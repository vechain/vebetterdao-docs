# Bot Signaling

Authorised apps can flag suspicious addresses such as bots, and scammers to help the VeBetter and other apps identify and protect against bad actors.

If a user is flagged incorrectly, **VeBetter** or selected apps can **reset their signal count** to clear their record.

* **Community Dashboard**: [Signal Admin Dashboard](https://vechain-energy.github.io/vebetterdao-signal-admin/)

## Endpoints

* **VeBetterPassport (Mainnet Address)**: `0x35a267671d8EDD607B2056A9a13E7ba7CF53c8b3`

Other apps and smart contracts can interact with the **Bot Signaling** module through these key functions in the `VeBetterPassport` contract:

```solidity
  interface IVeBetterPassport {
    ...
    function signalingThreshold() external view returns (uint256);
    function signaledCounter(address _user) external view returns (uint256);
    function appSignalsCounter(bytes32 _app, address _user) external view returns (uint256);
    function appTotalSignalsCounter(bytes32 app) external view returns (uint256);
    function signalUserWithReason(address _user, string memory reason) external;
    function resetUserSignalsByAppWithReason(address user, string memory reason) external;
    ...
  }
```

## How Bot Signaling Works

* **Checking Signals**:
  * You can query signal counts for users through the following methods:
    * `signaledCounter`: Returns the total number of times a user has been signaled across all applications
    * `appSignalsCounter`: Returns the number of times a user has been signaled by a specific application
  * Apps are free to decide how they interpret this number.
* **Threshold for Personhood**:
  * When using the `isPerson` function, a user's flagged count is compared to a set **threshold**.
  * If an address has been flagged **more than** the threshold amount, it will **fail** the personhood check.
    * You can check the current threshold amount by calling `signalingThreshold`

## Snippet

The following is a snippet of how to signal addresses by using the VeChain SDK.

```typescript
import {
  ThorClient,
  ProviderInternalBaseWallet,
  VeChainProvider,
} from '@vechain/sdk-network';
import { ErrorDecoder } from 'ethers-decode-error';
import passportAbi from './passport.json' assert { type: 'json' };

// get passport contract with signer from a helper function
const passport = await getPassportContract();

// what to signal
const addressToSignal = '0x0000000000000000000000000000000000000000';
const reason = 'test';
console.log('Address to signal is', addressToSignal, 'for', reason);

// log current signal count
const [counter] = await passport.read.signaledCounter(addressToSignal);
console.log('Signals received for address are', counter);

// send a signal
const tx = await passport.transact.signalUserWithReason(
  addressToSignal,
  reason
);

console.log('Signal sent in tx', tx.id);

async function getPassportContract() {
  // config sender
  const privateKey =
    '0x09b76a9e3c0f154ca2c6f9b9580ad5f6771493866553df98b10c38ddce987847';
  const address = '0x00351f449190a9C2A41e6989c98fe5fC5eB50000';
  const wallet = new ProviderInternalBaseWallet([
    { privateKey: Buffer.from(privateKey.slice(2), 'hex'), address },
  ]);

  // connect to network
  const thor = ThorClient.at('https://mainnet.vechain.org');

  const provider = new VeChainProvider(
    // Thor client used by the provider
    thor,

    // Internal wallet used by the provider (needed to call the getSigner() method)
    wallet,

    // Enable fee delegation
    false
  );

  const signer = await provider.getSigner(address);

  // contract config
  const passport = thor.contracts.load(
    // VePassport address
    '0x35a267671d8EDD607B2056A9a13E7ba7CF53c8b3',

    // the interface definition
    passportAbi,

    // origin signing transactions
    signer
  );

  return passport;
}
```

## Permission Request

If you need access to bot signaling functions, please **reach out to us.**

If you are the app admin&#x73;**,** you can **self-assign the required roles** using the following functions:

* **Granting or revoking `SIGNALER_ROLE`**:

```solidity
assignSignalerToAppByAppAdmin(bytes32 app, address user)
removeSignalerFromAppByAppAdmin(bytes32 app, address user)
```

**Note:** App admins are responsible for managing their team's permissions correctly.
