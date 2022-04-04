# API examples for Generic Assets

## Transferring funds
The most common thing to do with Generic Assets is to transfer some funds.
The two core assets in CENNZnet are:
* CPAY
* CENNZ

To make a CPAY transfer:

```js
let cpayId = (await api.tx.genericAsset.spendingAssetId());
let receiver = keyring.charlie.address;
let sender = keyring.alice;
let amount = 123456;
let txHash = await api.tx.genericAsset.transfer(cpayId, receiver, amount)
    .signAndSend(sender);
```

As described in the [subscriptions documentation](subscriptions), getting a `txHash` does not mean the transaction has been included in a block. In addition, we probably want to know if the transaction was successful. To do this, we use a subscription:

```js
// Sign and send a transfer from Alice to Bob
await api.tx.genericAsset
  .transfer(assetId, keyring.bob.address, 100000)
  .signAndSend(keyring.alice, ({ events = [], status }) => {
    console.log(`Current status is ${status.type}`);

    if (status.isFinalized) {
      console.log(`Transaction included at blockHash ${status.asFinalized}`);
      // Loop through Vec<EventRecord> to look for ExtrinsicSuccess
      events.forEach(({ phase, event }) => {
        console.log(`\t' ${phase}: ${event.section}.${event.method}:: ${event.data}`);
        if (event.section == 'system' && event.method == 'ExtrinsicSuccess') {
          console.log("Transaction is Successful");
        }
      });
    }
});
```

The expected output shows the lifecycle of the transaction, and events emitted:

```
Alice is transferring 1500000000 to Bob
Current status is Ready
Current status is InBlock
Current status is Finalized
Transaction included at blockHash 0x1a91533ba80cf52c25e457495e8a9f2cf5ebcb842d527055d4c9003dce491fd3
        ' {"ApplyExtrinsic":2}: genericAsset.Transferred:: [16001,"5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY","5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty",1500000000]
        ' {"ApplyExtrinsic":2}: system.ExtrinsicSuccess:: [{"weight":10000,"class":"Normal","paysFee":true}]
Transaction is Successful
```

## Checking balances
You may want to know how much of a certain asset you have in your account.

To check Alice's CENNZ balance is straight-forward:

```js
let cennz = (await api.query.genericAsset.stakingAssetId());
let account = keyring.alice.address;
let balance = await api.query.genericAsset.freeBalance(cennz, account);
```

If we want to know Alice's balance in real-time, we can use a subscription:

```js
let unsub = await api.query.genericAsset
.freeBalance(cennz, account, (balance) => {
  console.log(`Alice's CENNZ balance is: ${balance}`);
});
```

As new transactions are made, Alice's balance is updated in real-time:
```
Alice's CENNZ balance is: 1000000000000000000000000000
Alice's CENNZ balance is: 999998999999997480263496919
Alice's CENNZ balance is: 1000998999999997480263496919
```

The CENNZnet "Generic Asset" module manages the network's fungible tokens.  
Users can hold and transfer balances of tokens, while token owners may mint and burn token amounts according to their monetary policies.  
The native CENNZ and CPAY tokens are both instances of generic assets.  
Any user can create their own tokens on the network using this module.  
To learn more about how CENNZ and CPAY work in CENNZnet, refer to the [Token Economy](/Dapp-development/Guides/Token-Economy) document.  

For NFT (non-fungible tokens), use the dedicated [NFT](Runtime-modules/NFT) module. 

## Available Assets
Generic assets are uniquely identified by an integer Id e.g. on mainnet, CENNZ has asset id `1` and CPAY has asset id `2`.  
To find all the available assets on a CENNZnet network:
```js
const registeredAssets = await api.rpc.genericAsset.registeredAssets();
```

The response is a map of the available asset IDs and associated metadata. The metadata shows the symbol name and the number of decimal places for a balance of that asset.
```js
[
 (1, { symbol: 'CPAY', decimalPlaces: 4 }),
 (2, { symbol: 'CENNZ', decimalPlaces: 4 })
]
```

## Understanding Balances
Blockchains use unsigned integers for representing asset balances.  
This means balances are always counted in terms of the smallest indivisible unit.  
You can think of this like counting dollars in terms of cents.
Consider a generic asset with 2 decimal places:
A balance of `1` should be displayed to a user as `0.01`, while a whole token of this asset would be stored as `100`
and displayed to users as `1.00`.  
Dapps are responsible to ensure an asset's decimal places are respected when displaying balances.  

The following table shows this concept, the displayed balance changes with decimal places while the stored balance value does **not**.

|Balance|Decimal Places|Display|
|---|---|---|
|1|2|0.01|
|1|4|0.0001|
|1|18|0.000000000000000001|
|100|2|1.00|
|10000|4|1.0000|
|1e18|18|1.000000000000000000|

## Example usages

Listed below are the core functionalities of the Generic Asset module.

### Check account balance
```js
let assetId = 16000; // asset ID for CENNZ on Nikau testnet
let balance = await api.query.genericAsset.freeBalance(assetId, address);
```

### Transfer tokens
```js
await api.tx.genericAsset
    .transfer(assetId, destinationAddress, amount)
    .signAndSend(senderKeyPair);
```

### Checking total issuance of a token
```js
let totalIssuance = await api.query.genericAsset.totalIssuance(assetId);
```

### Creating a new token

This creates a new asset and configures the owner. The asset options allow the creator to set permissions and initial issuance of the token. Before issuing the token, make sure your account has enough CPAY to pay for the gas fee. For the TestNet, use the [Faucet](CENNZnet-infrastructures/CENNZnet-faucet) to issue tokens to your account.

```ts
import { Keyring } from '@polkadot/keyring';
// Add your owner keypair
const assetOwner = new Keyring({ type: 'sr25519' }).addFromSeed('<your private seed hex>');
// Number of whole tokens to issue
const initialIssuance = 1000000;
// Which address can mint, burn, and update permissions
const permissions = {
    update: {
        Address: assetOwner.address,
    },
    mint:  {
        Address: assetOwner.address,
    },
    burn:  {
        Address: assetOwner.address,
    },
};
const options = { initialIssuance , permissions };
// metadata of the new asset
const metadata = { symbol: 'TEST', decimalPlaces: 4, existentialDeposit: 0 };
// when the asset is created it will get this Id
let testAssetId = await api.query.genericAsset.nextAssetId();
// Create the asset
let unsub = await api.tx.genericAsset.create(
    assetOwner.address,
    options,
    metadata
).signAndSend(assetOwner, async ({ events, status }) => {
    // Add a callback to find out if the transaction was successful
    if (status.isInBlock) {
        for (const {event: {method, section, data}} of events) {
            console.log('Method:', method.toString());
            console.log('section:', section.toString());
        }
        console.log(`Transaction included at blockHash ${status.asInBlock}`);
        unsub();
    }
});
```

### Minting tokens

Mints an asset, increases its total issuance. Deposits the newly minted currency into target account. **Requires mint permissions**.

```js
await api.tx.genericAsset
    .mint(assetId, destinationAddress, amount)
    .signAndSend(ownerKeyPair);
```

### Burning tokens

Burns an asset, decreases its total issuance. Deducts the tokens from target account. **Requires burn permissions**.
```js
await api.tx.genericAsset
    .burn(assetId, address, amount)
    .signAndSend(ownerKeyPair);;
```

## API References
Check out the [Generic Assets API Example](CENNZnet-API/Examples/API-examples-Generic-Assets) for more detailed examples of common use cases!
