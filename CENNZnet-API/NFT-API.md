# NFT API

The NFT module is available from API version 1.4.0+.

This is the aggregated list of all API methods related to the NFT module. For the latest docs for particular versions of the API, please refer to the [docs in docs folder of the api.js repository](https://github.com/cennznet/api.js/tree/master/docs/cennznet).

## Token IDs
NFTs on CENNZnet are uniquely identifiable by a tuple of their collection, series, and serial number.  
`(collectionId, seriesId, serialNumber)`  
for example
`(5,1,1)`  
the token is from collection `5`, series `1` and has a serial number `1`  

## Unique or Series?
Unique tokens will forever be 1-of-1, Series tokens will be 1-of-n

To create a token which must always be unique use a `mintUnique` transaction  
To create tokens which have copies use a `mintSeries` transaction  

```js
let collectionId = 100;
let tokenReceiver = '5HbSE3cPakKMk7cRjtE58WF6RD57boEQ2aFzPcSxmHmRYUCi';
let uniqueTokenAttributes = [
  {'Text': 'My rare token'},
  {'Timestamp': 12345}
];
api.tx.nft.mintUnique(collectionId, tokenReceiver, uniqueTokenAttributes, null, null)

let seriesAttributes = [
  {'Text': 'My common token'},
  {'Timestamp': 12345}
];
let quantity = 100;
api.tx.nft.mintSeries(collectionId, quantity, tokenReceiver.address, seriesAttributes, metadataPath, null)
```

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
const {tokenId, tokenAttributes, owner} = tokenInfos[0];
```

[NFT APIs](https://raw.githubusercontent.com/cennznet/api.js/master/docs/cennznet/nft.md ':include :type=tsdoc')
