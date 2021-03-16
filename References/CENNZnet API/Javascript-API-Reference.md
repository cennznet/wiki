The CENNZnet blockchain features a rich API for developing your D'App.

The easiest way to get started is to follow our [API getting started guide](Interact-with-CENNZnet--using-the-API)

For a detailed reference on how the API works, see the [Polkadot API reference](https://polkadot.js.org/api/start/)

### API Object
The full Blockchain API is accessed by creating an API object:
```node
// Create the API and wait until ready
const api = await Api.create({
  provider: 'ws://localhost:9944'
});
```
The object needs:
* The websocket address (for testing locally, the default is `ws://localhost:9944`)

The import required to build the API object is:
```node
const { Api } = require('@cennznet/api');
```

### Sending Extrinsics with Doughnut

One of the key features of CENNZnet is delegated dispatch calls. This allows `Bob` to give `Alice` permissions using `Bob's` account.

When sending an extrinsic, doughnuts are attached when signing:

```node
const txHash = await api.tx.genericAsset
  .transfer(assetId, keyring.charlie.address, 1500000000)
  .signAndSend(keyring.alice, {doughnut: encoded_doughnut});
```

### Paying Transaction Fees Using a Non-Fee Asset

Transaction fees are usually paid in CentraPay, however fees can be paid in another token by using a `TransactionPayment` object:

```js
import {generateTransactionPayment} from '@cennznet/types/runtime/transaction-payment/TransactionPayment';
// ...
const transactionPayment = generateTransactionPayment({tip:0, assetId:CENNZ, maxPayment});
const unsub = await api.tx.genericAsset.transfer(CENNZ, bob.address, 10000)
  .signAndSend(alice,  {transactionPayment}, (result) => {
// ...etc
```

## CENNZnet Runtime Modules

### 1. Generic Asset

```node
api.tx.genericAsset.transfer(asset, to, amount)
```
Transfer `amount` of `asset` from the sender's free balance to the `to` account.

* `asset` - Asset Id of the asset to transfer
* `to` - Receiver of the asset
* `amount` - Amount to send

```node
api.tx.genericAsset.mint(asset, to, amount)
```
Mints `amount` of `asset` into account `to` and increases its total issuance. The sender must have `mint` permissions.

```node
api.tx.genericAsset.burn(asset, target, amount)
```
Burns `amount` of `asset` from `target` account and decreases its total issuance. The sender must have `burn` permissions.

```node
api.query.genericAsset.freeBalance(asset, who)
```
Get the free balance of `asset` held by account `who`.

```node
api.query.genericAsset.reservedBalance(asset, who)
```
Get the reserved balance of `asset` held by account `who`.

### 2. CENNZX Spot

```node
api.tx.cennzxSpot.buyAsset(recipient, asset_to_sell, asset_to_buy, buy_amount, maximum_sell)
```
Buy `asset_to_buy` with `asset_to_sell`. User specifies an exact `buy_amount` and a `maximum_sell` amount.

* `recipient` - Account to receive `buy_amount`, defaults to origin if `null`
* `asset_to_sell` - Asset ID to sell
* `asset_to_buy` - Asset ID to buy
* `buy_amount` - The amount of `asset_to_buy` to purchase
* `maximum_sell` - Maximum `asset_to_sell` we are willing to pay

```node
api.tx.cennzxSpot.sellAsset(recipient, asset_to_sell, asset_to_buy, sell_amount, minimum_buy)
```
Sell `asset_to_sell` for `asset_to_buy`. User specifies an exact `sell_amount` and a `minimum_buy` amount.

* `recipient` - Account to receive `buy_amount`, defaults to origin if `null`
* `asset_to_sell` - Asset ID to sell
* `asset_to_buy` - Asset ID to buy
* `sell_amount` - The amount of `asset_to_sell` to trade
* `minimum_buy` - Minimum `asset_to_buy` we are willing to trade for

```node
api.tx.cennzxSpot.addLiquidity(asset_id, minimum_liquidity, asset_amount, core_amount)
```
Deposit core asset and trade asset the current ratio to mint `liquidity`.
Returns amount of `liquidity` minted.

* `asset_id` - The trade asset ID
* `min_liquidity` - The minimum liquidity to add
* `asset_amount` - Amount of trade asset to add
* `core_amount` - Amount of core asset to add

```node
api.tx.cennzxSpot.removeLiquidity(asset_id, liquidity_to_withdraw, min_asset_withdraw, min_core_withdraw)
```
Burn exchange `liquidity` to withdraw core asset and trade asset at the current ratio

* `asset_id` - The trade asset ID
* `liquidity_to_withdraw` - Amount of user's liquidity to withdraw
* `min_asset_withdraw` - The minimum trade asset withdrawn
* `min_core_withdraw` - The minimum core asset withdrawn

```node
api.rpc.cennzx.buyPrice(asset_to_buy, buy_amount, asset_to_sell)
```
Get the price in `asset_to_sell` to buy `buy_amount` of `asset_to_buy`.

```node
api.rpc.cennzx.sellPrice(asset_to_sell, sell_amount, asset_to_buy)
```
Get the value of `asset_to_buy` when selling `sell_amount` of `asset_to_sell`.

```node
api.rpc.cennzx.liquidityValue(account_id, asset_id)
```
Get the value of liquidity owned by `account_id` for the exchange pool holding `asset_id`.

```node
api.rpc.cennzx.liquidityPrice(asset_id, liquidity_amount)
```
Get the cost in (`core_asset`, `trade_asset`) to buy `liquidity_amount` from the `asset_id` exchange pool.

### 3. Attestation

```node
api.tx.attestation.setClaim(holder, topic, value)
```
Make a claim of `value` on `topic` about `holder`.

* `holder` - Account ID of the claim `holder`
* `topic` - The claim topic encoded as a U256
* `value` - The claim value encoded as a U256

```node
api.tx.attestation.removeClaim(holder, topic)
```
Remove a previously made claim on `topic` about `holder`.

* `holder` - Account ID of the claim `holder`
* `topic` - The claim topic encoded as a U256