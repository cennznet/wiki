# CENNZX Exchange

CENNZX is a decentralised spot exchange built into the CENNZnet protocol.
It is based on the Uniswap contract's automated market maker algorithm.
It allows investors to create asset liquidity pools and earn fees from trades, while it allows traders
to make simple and easy trades between assets.

Because CENNZX is native to the CENNZnet protocol it is also integrated with network gas payment to provide transactors the ability to pay their fees in any liquid asset.

## Overview

### CENNZX vs Order Book Exchange
Unlike the order book trading exchange model, where there are buy orders and sell orders and the exchange matches up both sides for a trade, CENNZX uses on-chain reserve with automatic pricing and allows users to trade directly against its protocol, rather than other traders or market makers.

As market maker (or liquidity provider) on an order book exchange, you maintain a spread between two assets and slowly accumulate profits, but this usually requires you to have a large amount of initial capital. Whereas on CENNZX, it pulls liquidity from a number of providers, automates pricing and spread and fees are split proportional to their contribution - hence a much more user-friendly and decentralised experience. The protocol does not collect any platform fees. All fees are put back to the pool and collected when a liquidity provider exits from the pool.

### Price Determination / Exchange Rate
CENNZX uses a constant product market maker where investors lock up some amount of two assets, forming the assets' "Reserve Pool" for that exchange. In CENNZnet the two assets are always Cpay and one other asset.

The exchange rate is based on the ratio between the size of the two asset's Reserve Pool. For example, given a reserve pool of Asset _A_ and Cpay with 10,000 Asset _A_ and 1,000 Cpay, then the exchange rate will roughly be:

`10(Asset A) : 1(Cpay)`

The protocol will attempt to keep the product of two assets in the Reserve Pool constant. As the Reserve Pool of one of the two assets shrinks in size, that asset will become more valuable, and vice versa.

## Using the Exchange

### As a trader
A user can trade via `buyAsset` and `sellAsset` extrinsics, provided the Cpay-Asset pools exist for both the specified `asset_to_buy` and `asset_to_sell`. The cost to buy and sell assets can be checked at any time using the RPC calls `cennzx_buyPrice` and `cennzx_sellPrice`.

#### Trading Fee
A fee of 0.3% is charged for trading between a Cpay-Asset pair or vice-a-versa. For a Cennz->Cpay trade, 0.3% fee in Cennz is paid. For Cpay->Cennz trade, 0.3% fee in Cpay is paid. All fees are put back to the pool. Based on the liquidity provider's contribution to the pool, the fees are distributed among them.

_Note: When neither asset in a trade are Cpay, the trade is made by executing two exchanges: one between the user and the exchange, and one between the two exchange pools - this results in paying fees to each exchange pool._

### As a Liquidity Provider
If an asset does not yet have an exchange, it can be created and liquidity added via `addLiquidity`. Liquidity providers are required to deposit an equivalent value of both Cpay and the relevant asset into the exchange pool.

Alternatively, if an asset does have an exchange, a user can add liquidity via `addLiquidity`, but must provide liquidity in Cpay and the relevant asset at the ratio currently in the liquidity pool. The cost to add a certain volume of liquidity can be checked at any time using the RPC call `cennzx_liquidityPrice`.

At any time a liquidity provider may withdraw their liquidity via `removeLiquidity`. The liquidity provider will receive Cpay and the relevant asset as a proportion of the total pool holdings equivalent to the proportion of liquidity withdrawn versus the total pool liquidity. The holdings of a liquidity provider can be checked at any time using the RPC call `cennzx_liquidityValue`.

## Accessing CENNZX Exchange with the UI

To trade or provide liquidity on the exchange, go to [cennznet.io][cennznet_io_extrinsics], and select `cennzx` from the drop-down menu. There will be options to:
* `buyAsset` - buy a specific amount of a nominated `buy_asset` in exchange for a maximum amount of `sell_asset`
* `sellAsset` - sell a specific amount of a nominated `sell_asset` in exchange for at least a minimum amount of `buy_asset`
* `addLiquidity` - become a liquidity provider
* `removeLiquidity` - recover your holdings as a liquidity provider

![extrinsics-cennzx](../../assets/images/ui/extrinsics-cennzx.png)

To access pricing data, go to [cennznet.io][cennznet_io_toolbox], and select `cennzx` from the drop-down menu. There will be options to query:
* `buyPrice` - how much `sell_asset` is required to obtain a specific amount of `buy_asset`
* `sellPrice` - how much `buy_asset` would be obtained for a specific amount of `sell_asset`
* `liquidityPrice` - how much liquidity would cost in terms of a specific asset and Cpay on that asset's exchange
* `liquidityValue` - the value of an account's liquidity holding for a specific asset exchange
![](https://i.imgur.com/w9cZymt.png)

LiquidityValue - A user can check the value of any account's liquidity using an RPC call **liquidityValue** by specifying accountId and assetId. Lets say when initial liquidity of 10,000 is added by ALICE for asset XYZ, then this RPC call will return **[liquidity volume, core value, asset value]** - [10000, 10000, 10000]

LiquidityPrice - A user can check the price of an exchanges liquidity using an RPC call **liquidityPrice** by specifying assetId and liquidity to buy. This can beneficial for investor to know what combination of CORE-TRADE asset is required to invest in the liqudity pool. If initially pool of asset XYZ with core asset CPAY has 10,000 XYZ and 10,000 CPAY invested. The next investor can call this function to know for investing x amount of core asset, what would be trade asset required.. so lets say for 5000 CPAY, liquidityPrice will return **[core amount, trade asset amount]** - [5000, 5001]

## Accessing CENNZX Exchange with the API

The javascript API provides the same interfaces available through the UI.

```js
/// Extrinsics
api.tx.cennzx.buyAsset(recipient, assetToSell, assetToBuy, buyAmount, maximumSell);
api.tx.cennzx.sellAsset(recipient, assetToSell, assetToBuy, sellAmount, minimumBuy);
api.tx.cennzx.addLiquidity(assetId, minimumLiqudityToBuy, maximumAssetToPlace, coreAssetToPlace);
api.tx.cennzx.removeLiquidity(assetId, liquidityToWithdraw, minimumAssetToWithdraw, minimumCoreAssetToWithdraw);

/// RPC Calls
api.rpc.cennzx.buyPrice(assetToBuy, buyAmount, assetToSell);
api.rpc.cennzx.sellPrice(assetToSell, sellAmount, assetToBuy);
api.rpc.cennzx.liquidityValue(account, exchangeAsset);
api.rpc.cennzx.liquidityPrice(exchangeAsset, liquidityAmount);
```

_Note: if the calling account should always receive the traded token, we can set recipient to `null`:_

```js
api.tx.cennzx.buyAsset(null, assetToSell, assetToBuy, buyAmount, maximumSell);
```

To pay transaction fees in a nominated asset through the CENNZX exchange, refer to [this](https://github.com/cennznet/api.js/blob/develop/docs/examples/promise/12_chose_fee_asset_to_pay_tx_fee/index.js) example

## Calculating Price Slippage

The CENNZX buy/sell apis provide the trader protection against price slippage, if for example a large trade happens beforehand the price could shift significantly.  

Dapps can provide a simple slippage protection by querying current price and calculating the % slip allowed. For a buy trade, the user will want the price to not exceed current price + %slip while for a sale trade, the user will not want the price to slip below current price - %slip.

```ts
// buy
let currentPrice = (await api.rpc.buyPrice(assetIdToBuy, amountToBuy, assetIdToSell));
let twoPercentSlippage = currentPrice * 1.02;
await api.tx.cennzx.buyAsset(null, assetIdToSell, assetIdToBuy, amountToBuy, twoPercentSlippage).signAndSend(keypair);

// sell
let currentPrice = (await api.rpc.sellPrice(assetIdToSell, amountToSell, assetIdToBuy));
let twoPercentSlippage = currentPrice * 0.98;
await api.tx.cennzx.sellAsset(null, assetIdToSell, assetIdToBuy, amountToSell, twoPercentSlippage).signAndSend(keypair);
```

## Checking for available liquidity

To check if an asset has available liquidity in CENNZX check the balance of its liquidity pool.  
Recall liquidity pools are always between an asset and Cpay.  

```ts
// Query listed CENNZnet assets and check if liquidity is available
let liquidAssets = (await api.rpc.genericAsset.registeredAssets())
  .map(assetInfo => assetInfo[0])
  .filter(async (assetId) => await api.derive.cennzx.poolAssetBalance(assetId) > 0);
```

## Applications

### Paying a Merchant in a different asset

A common use case is when users need to pay merchants in an asset they don't hold. With CENNZX you can pay a merchant in one atomic transaction by specifying the merchant as the recipient in `buyAsset`.

### Seamless Fee Payment

Transaction fees can be paid in any asset, provided there's liquidity in the CENNZX. This is achieved by attaching a `FeeExchange` to an extrinsic which will then seamlessly convert the elected asset into CentraPay for fees. This streamlines user onboarding and in-app experience. This feature is also available for paying gas on CENNZnet smart contracts.

In the "transaction payment" example provided in the section above, the ```transactionPayment``` object created contains ways to pay for the transaction fee for the ```transfer``` extrinsics call. ```assetId:CENNZ``` can be replaced with any asset ID, and the CENNZX Exchange will automatically convert it into the correct asset for fee payment.

### Further Reading
For code examples refer to the @cennznet/api test suite:   
https://github.com/cennznet/api.js/blob/develop/packages/api/src/derives/cennzx/cennzxTrade.e2e.ts

***

The CENNZX is inspired by the [Uniswap protocol][uniswap]

[cennznet_io_extrinsics]: https://cennznet.io/#/extrinsics
[cennznet_io_toolbox]: https://cennznet.io/#/toolbox
[uniswap]: https://uniswap.org/whitepaper.pdf

