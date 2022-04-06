# NFT Module 🃏

The CENNZnet NFT module allows users to create custom NFTs using only the javascript API and simple transactions, without the need for smart contracts.  

The module has an integrated marketplace allowing NFT owners to:  
- define royalty schemes for secondary sales of tokens
- create fixed price sales  
- create auction sales  

The [NFT API](api-references/NFT-API) documention describes the interface of this module.

## Token Attributes & Example Queries

Supported NFT data types are listed here:
```rust
i32
u8
u16
u32
u64
u128
Bytes32
Bytes
String
Hash
Timestamp
Url
```
Some are just nice aliases which give meaning to the data e.g. Url is a String.

Query a collection's tokens
```js
let collectionId = 0;
let collectionName = (await api.query.nft.collectionName(collectionId));
console.log(collectionName);
// Get collection tokens
let tokens = (await api.derive.nft.tokenInfoForCollection(collectionId));
console.log(tokens);

// get individual token info by Id
let collectionId = 0;
let seriesId = 0;
let serialNumber = 0;
let token0 = (await api.derive.nft.tokenInfo(tokenId));
console.log(token0.toHuman());

// All token Ids owned by an account in a given collection
let owner = '5FLSigC9HGRKVhB9FiEo4Y3koPsNmBmLJbpXg2mp1hXcS59Y';
let owned = (await api.query.nft.collectedTokens(collectionId, owner));

// Get all listings of tokens for a given collection.
const listedTokenInfo = await api.derive.nft.openCollectionListingsV2(collectionId); 
/*
 [{
    listingId: 12, 
    tokens: [
        {owner: '0x..', tokenId: [16,0,1]},
        {owner: '0x..', tokenId: [16,0,2]}
    ]
}];
*/
```
