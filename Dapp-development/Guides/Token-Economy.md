# Token Economy

CENNZnet is built with a native multi-token economy, similar to the ERC20 specification aka *generic assets*. Any account on CENNZnet can hold a balance of one or more of these assets.


### Staking and Spending Assets (CENNZ + CPAY)

Two assets on CENNZnet have special roles in the economy. CENNZ is the network staking token, while CPAY is the network fee token.
CENNZ is used to participate in network staking duties and governance.

For more information on the benefits and economics of the dual token economy, please refer to our article [The dual token economy: explaining CENNZ and CPAY](https://medium.com/centrality/the-dual-token-economy-explaining-cennz-and-cpay-c9bd3c5b7430).

As a side-effect an account's CENNZ balance may be partially locked e.g.
an account has staked some of it's CENNZ and will not be able to transfer those tokens, until it has exited the staking system.

The current locks on an account's CENNZ can be found using:
```js
await api.query.genericAsset.locks(<address>); // return all CENNZ locks
```

These funds will still appear under the account's 'free CENNZ balance' but will not be transferrable.

In contrast, the spending token CPAY, cannot be locked. It is used to pay for transaction fees.

If in doubt, you can query the staking and spending asset IDs from the chain:
```js
await api.query.genericAsset.stakingAssetId(); // 1 on MainNet
await api.query.genericAsset.spendingAssetId(); // 2 on MainNet
```

### Querying available assets
Assets must be created and registered before accounts can hold them.

The js API provides a method to query available assets on CENNZnet:
```js
const registeredAssets = await api.rpc.genericAsset.registeredAssets();
```

The response is a map of the available asset IDs and associated metadata. The metadata shows the symbol name and the number of decimal places for a balance of that asset.
```js
[
 (1, { symbol: 'CPAY', decimalPlaces: 4}),
 (2, { symbol: 'CENNZ', decimalPlaces: 4})
],
```

### Displaying and querying account balances

Quering an account's balance requires the asset ID in question and the address of the account.
```js
let balance = await api.query.genericAsset.freeBalance(<asset id>, <address>);
```

Asset decimal places should be respected when displaying balances of that asset. CENNZ and CPAY are both 4 decimal places.
For example, a raw "wei" value of `10000` CENNZ asset should be rendered as `1.0000 CENNZ` to users.

### Transferring assets

Transferring assets between accounts simply requires an amount and asset ID to be specified.
```js
// Create the transaction
let tx = await api.tx.genericAsset.transfer(<asset id>, <destination address>, <amount>);

// Sign it and send it to the connected network
await tx.signAndSend(senderKeypair);
```