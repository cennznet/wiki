The storage on CENNZnet is always changing. The CENNZnet API provides a subscription mechanism; this allows your D'App to update as soon as the storage that you care about changes.

General details of subscription mechanisms are described in the [Polkadot API docs](https://polkadot.js.org/api/start/); this guide provides a summary and examples for CENNZnet.

## Query Subscriptions

In the API getting started guide, we get a user's balance by calling:

```js
let balance = await api.query.genericAsset
  .balance(assetId, keyring.alice.address);
```

If we want to subscribe to Alice's `freeBalance`, we add a callback function
* The callback function will be called every time Alice's free balance of `assetId` is updated.
* The query now returns an `unsub` callback; we should call this when we no longer need our subscription.

```js
let unsub = await api.query.genericAsset
  .balance(assetId, keyring.alice.address, (balance) => {
  console.log(`Alice has ${balance} in asset(${assetId})`);
});
```

All queries can be turned into subscriptions by adding a callback function to the end of them.

## Transaction Subscriptions

In the API getting started guide, we transfer assets from Alice to Bob by sending:

```js
const txHash = await api.tx.genericAsset
  .transfer(assetId, keyring.bob.address, 1500000000)
  .signAndSend(keyring.alice);
```

The result is a hash of the transaction, but this does not guarantee that the transaction will be included - only that it has been submitted.

To monitor and determine the status of a transaction, we use a subscription:

```js
const unsub = await api.tx.genericAsset
  .transfer(assetId, keyring.bob.address, 1500000000)
  .signAndSend(alice, (result) => {
    console.log(`Current status is ${result.status}`);

    if (result.status.isInBlock) {
      console.log(`Transaction included at blockHash ${result.status.asInBlock}`);
    } else if (result.status.isFinalized) {
      console.log(`Transaction finalized at blockHash ${result.status.asFinalized}`);
      unsub();
    }
  });
```

Transactions can be in the following states:
* `ready` this means the transaction has been submitted to the node
* `broadcast` the node has broadcast the transaction to other nodes (you may not see this if you are testing with a single node)
* `inBlock` an author node has included the transaction in a block
* `finalized` the block the transaction was included in has been finalized - we usually want to get to this point before concluding that the transaction is complete

A finalized transaction does not mean that it was successful - it only means it was included in a finalized block. If we need to know whether a transaction was successful, then check the event record:

```js
const unsub = await api.tx.genericAsset
  .transfer(assetId, keyring.bob.address, 1500000000)
  .signAndSend(keyring.alice, ({ events = [], status }) => {
    console.log(`Current status is ${status.type}`);

    if (status.isFinalized) {
      console.log(`Transaction included at blockHash ${status.asFinalized}`);

      // Loop through Vec<EventRecord> to display all events
      events.forEach(({ phase, event: { data, method, section } }) => {
        console.log(`\t' ${phase}: ${section}.${method}:: ${data}`);
      });

      unsub();
    }
  });
```

## Event Subscriptions

Runtime modules may publish events when something changes. For example, the `genericAssets` module emits:
```rust
Transferred(asset, from, to, amount)
```
when an `amount` of `asset` is transferred `from` an account `to` an account.

We can subscribe to events using:
```js
// Subscribe to system events via storage
api.query.system.events((events) => {
  console.log(`\nReceived ${events.length} events:`);

  // Loop through the Vec<EventRecord>
  events.forEach((record) => {
    // Extract the phase, event and the event types
    const { event, phase } = record;
    const types = event.typeDef;

    // Show what we are busy with
    console.log(`\t${event.section}:${event.method}:: (phase=${phase.toString()})`);
    console.log(`\t\t${event.meta.documentation.toString()}`);

    // Loop through each of the parameters, displaying the type and data
    event.data.forEach((data, index) => {
      console.log(`\t\t\t${types[index].type}: ${data.toString()}`);
    });
  });
});
```

If we wanted to filter out the `generticAssets:Transferred` event we might write:
```js
if (event.section == `genericAssets` && event.method == `Transferred`) {
  // do something with event.data here
}
```

## RPC Subscriptions

`api.rpc` provide some additional subscriptions for specific RPC calls, they function the same way as the above subscriptions:
* we pass in a callback for handling new data
* the `await` call returns an `unsub` function to call when we want to stop receiving data

```js
const unsubHeads = await api.rpc.chain.subscribeNewHeads((lastHeader) => {
  console.log(`${chain}: last block #${lastHeader.number} has hash ${lastHeader.hash}`);
});
// Unsubscribe one minute later
setTimeout(unsubHeads, 60000);
```
