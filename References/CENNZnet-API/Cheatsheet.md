# Cheatsheet

## Get the latest block height
with curl:
```bash
# returns block number hex encoded
curl -H "Content-Type: application/json" \
     -d '{ "jsonrpc":"2.0", "method":"chain_getBlock", "params":[],"id":1 }' http://example.com:9933 | jq .result.block.header.number
```

with the [@cennznet/api](https://www.npmjs.com/package/@cennznet/api):
```js
let block = (await api.rpc.chain.getBlock());
```

Or subscribe:
```js
// use `subscribeFinalizedHeads` for finalized only
await api.rpc.chain.subscribeNewHeads((header) => {
  console.log(header.toJSON());
})
```

## Find transactions in a block

If the block is in node state try:
```bash
curl -H "Content-Type: application/json" \
     -d '{ "jsonrpc":"2.0", "method":"chain_getBlock", "params":[<block number>],"id":1 }' http://example.com:9933 | jq .result.block.extrinsics
```

alternatively, our block explorer provides a block API
```bash
curl -H "Content-Type: application/json" -d '{"block_num": 100000 }' https://scan-azalea.onfinality.me/api/scan/block | jq .data.extrinsics
```

example output:
```json
[
  {
    "account_id": "5C4hrfjw9DjXZTzV3MwzrrAr9P1MJhSrvWGWqi1eSuyUpnhM",
    "block_num": 100000,
    "block_timestamp": 1586216540,
    "extrinsic_index": "100000-0",
    "extrinsic_hash": "0x0296798d64cba412b3f891b40be9cb0421d2758ef15b5151a7998a4c271702f2",
    "call_module": "timestamp",
    "call_module_function": "set",
    "params": "[{\"name\":\"now\",\"type\":\"Compact<Moment>\",\"value\":1586216540000}]",
    "fee": "0",
    "tip": null,
    "finalized": true,
    "success": true,
    "signature": null
  }
]
```


## Check a transaction for success

To verify a transaction when sending use the [subscription](References/CENNZnet-API/Subscriptions?id=transaction-subscriptions) api.

To verify a transaction hash is in the chain, use our block explorer (UNCover) API

```bash
curl -H "Content-Type: application/json" \
  -d '{"hash": "0xbe3781892d397395afdde9086cc0028426612468bd37841241284e92facf34ea" }' https://scan-azalea.onfinality.me/api/scan/extrinsic | jq .data.success
```

## Offline Transaction Signing
This project provides an example: https://github.com/polkadot-js/tools/blob/287420490d586c61702c6836b0906e4f64869a92/packages/signer-cli/src/cmdSendOffline.ts#L52-L77

You must collect metadata material from the node to create valid transactions.
This metadata material will need to be updated from time to time inorder for the transaction signature to be accepted after network updates etc.

## Query an account's balance

See the [generic asset's guide](Dapp-development/Guides/Token-Economy?id=displaying-and-querying-account-balances)

## Estimate a transaction's fees

```typescript
// create tx with args and sender
// query its 'paymentInfo'
const info = await api.tx.genericAsset
  .transfer(16000, recipientAddress, 123)
  .paymentInfo(senderKeyPair);

// log relevant info, partialFee is Balance, estimated for current
console.log(`
  class=${info.class.toString()},
  weight=${info.weight.toString()},
  partialFee=${info.partialFee.toHuman()}
`);
```

CENNZnet allows paying for transaction fees using any liquidity pool on CENNZX
```typescript
api.derive.fees.estimateFee({extrinsic, userFeeAssetId: cpayAssetId});
```

also see: https://polkadot.js.org/docs/api/cookbook/tx/#how-do-i-estimate-the-transaction-fees