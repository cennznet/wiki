Doughnuts are Proofs of Delegation between two or more cryptographic keypairs. Doughnuts let us prove that one address delegates something to another address.

For example, if `Alice` wants to let `Bob` use her account, but only to send tokens to `Charlie`. This could be achieved using a doughnut by doing the following:
* `Alice` writes a "Pact" which permissions `Charlie` as the recipient of tokens.
* `Alice` creates a "CENNZnut" which sets:
  * "generic-asset" as a permissioned module
  * "transfer" as a permissioned method for "generic-asset"
  * the "Pact" for `Charlie` as the constraints for "transfer"
* `Alice` creates a doughnut which sets:
  * `Alice` as the issuer
  * `Bob` as the holder
  * A "cennznet" permission domain with the "CENNZnut" as the payload of that domain
* `Alice` signs the doughnut and gives it to `Bob`
* When `Bob` wants to send funds to `Charlie` with `Alice`'s account, `Bob` should attach the doughnut when signing and sending an extrinsic.

In CENNZnet the Doughnut protocol requires a "cennznet" permission domain with a "CENNZnut" embedded. We optionally embed "Pact" constraints within the "CENNZnut" to further limit the actions which can be taken with delegated permissions.

Details of the Doughnut and CENNZnut are captured in the [Doughnut Paper](https://github.com/cennznet/doughnut-paper).

Details of Pact is available in the [Pact Documentation](https://github.com/cennznet/pact/tree/master/design/pact)

## Using Javascript SDK to create a doughnut

**TODO**: The SDK to build a CENNZnet doughnut is still in progress. 

## Send a doughnut to CENNZnet using the API

### 1. Create a signed and encoded doughnut

**TODO**

### 2. Add the encoded doughnut to an extrinsic

We will do a `genericAsset.transfer` and add the encoded doughnut in the option parameter.

```js
// Create the API and wait until ready
const api = await Api.create({ provider: "ws://localhost:9944" });

// Send transfer extrinsic with doughnut
const options = { doughnut: doughnut.encode() };
const txHash = await api.tx.genericAsset
  .transfer(assetId, keyring.charlie.address, 1500000000)
  .signAndSend(keyring.bob, options);
```

## Send a doughnut to CENNZnet using the UI
**TODO** Not available in UI yet