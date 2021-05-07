# CENNZnet API example usages

The CENNZnet blockchain features a rich API for developing your DApp. This document shows some example usages of the CENNZnet API that interacts with different runtime modules. To learn more about available runtime modules, check the [List of CENNZnet modules](References/Runtime-modules/List-of-cennet-modules).

Before you start here, please follow the [Getting started with the CENNZnet API guide](Dapp-development/Guides/Getting-started-with-the-CENNZnet-API) and the [CENNZnet API Overview](References/CENNZnet-API/CENNZnet-API-Overview).

### API Object
The full Blockchain API is accessed by creating an API object:
```js
// Create the API and wait until ready
const api = await Api.create({
  provider: 'ws://localhost:9944'
});
```
The object needs:
* The websocket address (for testing locally, the default is `ws://localhost:9944`)

The import required to build the API object is:
```js
const { Api } = require('@cennznet/api');
```

### Sending Extrinsics with Doughnut

One of the key features of CENNZnet is delegated dispatch calls. This allows `Bob` to give `Alice` permissions using `Bob's` account.

When sending an extrinsic, doughnuts are attached when signing:

```js
const txHash = await api.tx.genericAsset
  .transfer(assetId, keyring.charlie.address, 1500000000)
  .signAndSend(keyring.alice, {doughnut: encoded_doughnut});
```

### Paying Transaction Fees Using a Non-Fee Asset

Transaction fees are usually paid in CPay, however fees can be paid in another token by using a `TransactionPayment` object:

```js
import {generateTransactionPayment} from '@cennznet/types/runtime/transaction-payment/TransactionPayment';
// ...
const transactionPayment = generateTransactionPayment({tip:0, assetId:CENNZ, maxPayment});
const unsub = await api.tx.genericAsset.transfer(CENNZ, bob.address, 10000)
  .signAndSend(alice,  {transactionPayment}, (result) => {
// ...etc
```
