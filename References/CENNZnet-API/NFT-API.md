# NFT API

## Storage methods
https://github.com/cennznet/api.js/blob/develop/docs/cennznet/storage.md#nft

## RPC methods
https://github.com/cennznet/api.js/blob/develop/docs/cennznet/rpc.md#nft

## Derived methods
Derived methods are helper methods written on the API side, to perform additional processing on the storage and RPC methods.

```js
import { DeriveTokenInfo } from '@cennznet/api/derives/nft/types';

/**
 * @description Retrieve the list of all tokens in a collection
 */
export function tokenInfoForCollection(instanceId: string, api: ApiInterfaceRx): () => Observable<DeriveTokenInfo[]> {};

// Usage
const tokenInfos: DeriveTokenInfo[] = await api.derive.nft.tokenInfoForCollection(collectionId);
const {tokenId, tokenDetails, owner} = tokenInfos[0];
```
## Extrinsic methods
https://github.com/cennznet/api.js/blob/develop/docs/cennznet/extrinsics.md#nft


## Errors
https://github.com/cennznet/api.js/blob/develop/docs/cennznet/errors.md#nft

## Events
https://github.com/cennznet/api.js/blob/develop/docs/cennznet/events.md#nft
