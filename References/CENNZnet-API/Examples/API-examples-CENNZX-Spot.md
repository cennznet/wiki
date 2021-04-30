# API examples for CENNZX Spot

The examples used here will focus on CENNZ and CPay assets, thier asset IDs are:
* CPay - id `16000`
* CENNZ - id `16001`

## Trading

Trading on CENNZXSpot allow you to either `buy` or `sell` a specific `amount` of asset at a market rate.

### Buying
When you want to buy a specific `amount` of an asset, there are two useful calls:
* `buyPrice`
* `buyAsset`

To find out the current `buy` price of an asset use the RPC API. For example, if we wanted to buy `5000 CPay` using `CENNZ`
```js
let CPay = 16000;
let cennz = 16001;
let amount = 5000;

let price = await api.rpc.cennzx.buyPrice(CPay, amount, cennz);

console.log(`${amount} CPay can be bought for ${price} CENNZ`);
```
Assuming there is some liquidity in the CENNZ/CPay pool, we should see something like this:
```
5000 CPay can be bought for 5016 CENNZ
```
Unfortunately, `api.rpc.cennzx` calls cannot be subscribed to directly, but we could achieve this by subscribing to `cennzxSpot` related events - or even storage changes on the exchangePool's balance (advanced).

If we decide that the price of CPay is fair we can execute a trade:
```js
let maxCennzToSell = 5016;

let txHash = await api.tx.cennzxSpot
  .buyAsset(null, cennz, CPay, amount, maxCennzToSell)
  .signAndSend(keyring.alice);
```
The `null` argument specifies that the `recipient` of the trade is the `trader`; see below for an example where we specify another account to receive the traded asset.

### Selling
When you want to sell a specific `amount` of an asset, there are two useful calls:
* `sellPrice`
* `sellAsset`

To find out the current `sell` price of an asset use the RPC API. For example, if we wanted to sell `5000 CENNZ` for `CPay`
```js
let CPay = 16000;
let cennz = 16001;
let amount = 5000;

let price = await api.rpc.cennzx.sellPrice(cennz, amount, CPay);

console.log(`${amount} CENNZ can be sold for ${price} CPay`);
```
Assuming there is some liquidity in the CENNZ/CPay pool, we should see something like this:
```
5000 CENNZ can be sold for 4985 CPay
```

If we decide that the amount of CPay is fair we can execute a trade:
```js
let minCPayToBuy = 4985;

let txHash = await api.tx.cennzxSpot
  .buyAsset(null, cennz, CPay, amount, minCPayToBuy)
  .signAndSend(keyring.alice);
```

### Transferring an asset that you don't own to another account
The CENNZX Spot exchange allows us to pay someone else in an asset we don't own.

For example, say that Alice has no `CPay`, but Dave requires `1000 CPay` for his services. Alice can use CENNZX Spot to pay Dave using Alice's `CENNZ` balance:

```js
let CPay = 16000;
let cennz = 16001;
let amount = 1000;
let max_cennz_to_sell = 1500;
let recepient = keyring.dave.address;

let txHash = await api.tx.cennzxSpot
  .buyAsset(recepient, cennz, CPay, amount, max_cennz_to_sell)
  .signAndSend(keyring.alice);
```

Usually, for trade-critical transactions, you will want to ensure that the trade completes successfully as well. See the [Generic Assets API Examples](API-examples-Generic-Assets) for how to subscribe to an extrinsics outcome.

## Investing
Putting assets into the exchange means that your assets will grow as fees are accumulated from traders using the exchange.

You could first check the liquidity price of putting assets into a pool. Say we wanted to put CENNZ and CPay into a pool:
```js
let cennz = 16001;
let liquidityAmount = 1000;
let (CPayPrice, cennzPrice) = api.rpc.cennzx.liquidityPrice(cennz, liquidityAmount);

console.log(`To buy ${liquidityAmount} liquidity from the CENNZ exchange will cost:\n\t ${CPayPrice} CPay\n\t ${cennzPrice} CENNZ`);
```
We should see something like this:
```
To buy 1000 liquidity from the CENNZ exchange will cost:
   1001 CPay
   1003 CENNZ`
```

To add liquidity into a pool:
```js
let txHash = await api.tx.cennzxSpot
  .addLiquidity(cennz, liquidityAmount, cennzPrice, CPayPrice)
  .signAndSend(keyring.alice);
```

You can check the value of your held liquidity at any time:
```js
let (liquidityAmount, CPayValue, cennzValue) = api.rpc.cennzx.liquidityValue(keyring.alice.address, cennz);

console.log(`Holding ${liquidityAmount} liquidity in the CENNZ exchange. Current value:\n\t ${CPayValue} CPay\n\t ${cennzValue} CENNZ`);
```
We should see something like this:
```
Holding 1000 liquidity in the CENNZ exchange. Current value:
   1001 CPay
   1003 CENNZ`
```

Finally, to convert liquidity back into assets, we use:
```js
let txHash = await api.tx.cennzxSpot
  .removeLiquidity(cennz, liquidityAmount, cennzValue, CPayValue)
  .signAndSend(keyring.alice);
```
