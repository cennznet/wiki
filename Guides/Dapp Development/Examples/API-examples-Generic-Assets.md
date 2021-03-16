## Transferring funds
The most common thing to do with Generic Assets is to transfer some funds.
The two core assets in CENNZnet are:
* CPAY
* CENNZ

To make a CPAY transfer:

```javascript
let cpayId = (await api.tx.genericAsset.spendingAssetId());
let receiver = keyring.charlie.address;
let sender = keyring.alice;
let amount = 123456;
let txHash = await api.tx.genericAsset.transfer(cpayId, receiver, amount)
    .signAndSend(sender);
```

As described in the [subscriptions documentation](subscriptions), getting a `txHash` does not mean the transaction has been included in a block. In addition, we probably want to know if the transaction was successful. To do this, we use a subscription:

```js
let waiting = true;
let success = false;

console.log(`Alice is transferring 1_500_000_000 to Bob`);

// Sign and send a transfer from Alice to Bob
const unsub = await api.tx.genericAsset
.transfer(assetId, keyring.bob.address, 1500000000)
.signAndSend(keyring.alice, ({ events = [], status }) => {
  console.log(`Current status is ${status.type}`);

  if (status.isFinalized) {
    console.log(`Transaction included at blockHash ${status.asFinalized}`);

    // Loop through Vec<EventRecord> to look for ExtrinsicSuccess
    events.forEach(({ phase, event }) => {
      console.log(`\t' ${phase}: ${event.section}.${event.method}:: ${event.data}`);
      if (event.section == 'system' && event.method == 'ExtrinsicSuccess') {
        success = true;
      }
    });
    unsub();
    waiting = false;
  }
});

// Wait until the extrinsic has been finalized
// (note: don't do this in production, sometimes tx are never finalized)
while(waiting) {
  await sleepMs(100);
}

if (success) {
  console.log("Transaction is Successful");
} else {
  console.log("Transaction Failed")
}
```

The above example uses a custom sleepMs function, which can be defined as follows:

```js
function sleepMs(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
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

```javascript
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

## Finding Available Assets
An RPC query is provided to find the available generic assets on a CENNZnet blockchain.
These assets have been minted/issued on the chain.

```js
const registeredAssets = await api.rpc.genericAsset.registeredAssets();
```

The response is a map of the available asset IDs and associated metadata.
At the time of writing the metadata is the asset / symbol name and the number of decimal places for a balance of that asset.
```js
[
 (2, { symbol: 'CPAY', decimalPlaces: 4}),
 (1, { symbol: 'CENNZ', decimalPlaces: 4})
],
```