# NFT API

This is the aggregated list of all API methods that's related to the NFT module. For the latest docs for particular versions of the API, please refer to the [docs in docs folder of the api.js repository](https://github.com/cennznet/api.js/tree/develop/docs/cennznet).

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

## Storage methods
[Docs in Github](https://github.com/cennznet/api.js/blob/develop/docs/cennznet/storage.md#nft)
 
### collectionMetadataURI(`CollectionId`): `MetadataURI`
- **interface**: `api.query.nft.collectionMetadataURI`
- **summary**:   Map from collection to a base metadata URI for its token's offchain attributes 
 
### collectionOwner(`CollectionId`): `Option<AccountId>`
- **interface**: `api.query.nft.collectionOwner`
- **summary**:   Map from collection to owner address 
 
### collectionRoyalties(`CollectionId`): `Option<RoyaltiesSchedule>`
- **interface**: `api.query.nft.collectionRoyalties`
- **summary**:   Map from collection to it's defacto royalty scheme 
 
### collectionSchema(`CollectionId`): `Option<NFTSchema>`
- **interface**: `api.query.nft.collectionSchema`
- **summary**:   Map from collection to its onchain schema definition 
 
### listingEndSchedule(`BlockNumber, (CollectionId,TokenId)`): `Option<()>`
- **interface**: `api.query.nft.listingEndSchedule`
- **summary**:   Block numbers where listings will close. It is `Some` if at block number, (collection id, token id) is listed and scheduled to close. 
 
### listings(`CollectionId, TokenId`): `Option<Listing>`
- **interface**: `api.query.nft.listings`
- **summary**:   NFT sale/auction listings. keyed by collection id and token id 
 
### listingWinningBid(`CollectionId, TokenId`): `Option<(AccountId,Balance)>`
- **interface**: `api.query.nft.listingWinningBid`
- **summary**:   Winning bids on open listings. keyed by collection id and token id 
 
### nextTokenId(`CollectionId`): `TokenId`
- **interface**: `api.query.nft.nextTokenId`
- **summary**:   The next available token Id for an NFT collection 
 
### tokenAttributes(`CollectionId, TokenId`): `Vec<NFTAttributeValue>`
- **interface**: `api.query.nft.tokenAttributes`
- **summary**:   Map from (collection, token) to it's attributes (as defined by schema) 
 
### tokenIssuance(`CollectionId`): `TokenId`
- **interface**: `api.query.nft.tokenIssuance`
- **summary**:   The total number an NFT collection in circulation (excludes burnt tokens) 
 
### tokenOwner(`CollectionId, TokenId`): `AccountId`
- **interface**: `api.query.nft.tokenOwner`
- **summary**:   Map from (collection, token) to it's owner 
 
### tokenRoyalties(`CollectionId, TokenId`): `Option<RoyaltiesSchedule>`
- **interface**: `api.query.nft.tokenRoyalties`
- **summary**:   Map from a token to it's royalty scheme 


## RPC methods
[Docs in Github](https://github.com/cennznet/api.js/blob/develop/docs/cennznet/rpc.md#nft)

### collectedTokens(collection: `CollectionId`, address: `Address`): `Vec<TokenId>`
- **interface**: `api.rpc.nft.collectedTokens`
- **jsonrpc**: `nft_collectedTokens`
- **summary**: Get the tokens owned by an address in a certain collection


## Extrinsic methods
[Docs in Github](https://github.com/cennznet/api.js/blob/develop/docs/cennznet/extrinsics.md#nft)

### auction(collection_id: `CollectionId`, token_id: `TokenId`, payment_asset: `AssetId`, reserve_price: `Balance`, duration: `Option<BlockNumber>`)
- **interface**: `api.tx.nft.auction`
- **summary**:   Sell NFT on the open market to the highest bidder Caller must be the token owner 

  - `reserve_price` winning bid must be over this threshold

  - `payment_asset` fungible asset Id to receive payment with

  - `duration` length of the auction (in blocks), uses default duration if unspecified
 
### bid(collection_id: `CollectionId`, token_id: `TokenId`, amount: `Balance`)
- **interface**: `api.tx.nft.bid`
- **summary**:   Place a bid on an open auction 

  - `amount` to bid (in the seller's requested payment asset)
 
### burn(collection_id: `CollectionId`, token_id: `TokenId`)
- **interface**: `api.tx.nft.burn`
- **summary**:   Burn an NFT 🔥 Caller must be the token owner 
 
### cancelSale(collection_id: `CollectionId`, token_id: `TokenId`)
- **interface**: `api.tx.nft.cancelSale`
- **summary**:   Close a sale or auction Requires no successful bids have been made for the auction. Caller must be the token owner 
 
### createCollection(collection_id: `CollectionId`, schema: `NFTSchema`, metadata_uri: `Option<MetadataURI>`, royalties_schedule: `Option<RoyaltiesSchedule>`)
- **interface**: `api.tx.nft.createCollection`
- **summary**:   Create a new NFT collection The caller will be come the collection' owner `collection_id`- 32 byte utf-8 string `schema` - for the collection `royalties_schedule` - defacto royalties plan for secondary sales, this will apply to all tokens in the collection by default. 
 
### createToken(collection_id: `CollectionId`, owner: `AccountId`, attributes: `Vec<NFTAttributeValue>`, royalties_schedule: `Option<RoyaltiesSchedule>`)
- **interface**: `api.tx.nft.createToken`
- **summary**:   Issue a new NFT `owner` - the token owner `attributes` - initial values according to the NFT collection/schema `royalties_schedule` - optional royalty schedule for secondary sales of _this_ token, defaults to the collection config Caller must be the collection owner 
 
### directPurchase(collection_id: `CollectionId`, token_id: `TokenId`)
- **interface**: `api.tx.nft.directPurchase`
- **summary**:   Buy an NFT for its listed price, must be listed for sale 
 
### directSale(collection_id: `CollectionId`, token_id: `TokenId`, buyer: `Option<AccountId>`, payment_asset: `AssetId`, fixed_price: `Balance`, duration: `Option<BlockNumber>`)
- **interface**: `api.tx.nft.directSale`
- **summary**:   Sell an NFT to specific account at a fixed price `buyer` optionally, the account to receive the NFT. If unspecified, then any account may purchase `asset_id` fungible asset Id to receive as payment for the NFT `fixed_price` ask price `duration` listing duration time in blocks Caller must be the token owner 
 
### setOwner(collection_id: `CollectionId`, new_owner: `AccountId`)
- **interface**: `api.tx.nft.setOwner`
- **summary**:   Set the owner of a collection Caller must be the current collection owner 
 
### transfer(collection_id: `CollectionId`, token_id: `TokenId`, new_owner: `AccountId`)
- **interface**: `api.tx.nft.transfer`
- **summary**:   Transfer ownership of an NFT Caller must be the token owner 

## Errors
[Docs in Github](https://github.com/cennznet/api.js/blob/develop/docs/cennznet/errors.md#nft)
### BidTooLow
- **summary**:   Auction bid was lower than reserve or current highest bid 
 
### CollectionIdExists
- **summary**:   A collection with the same ID already exists 
 
### CollectionIdInvalid
- **summary**:   Given collection ID is not valid utf-8 
 
### InternalPayment
- **summary**:   Internal error during payment 
 
### MaxAttributeLength
- **summary**:   Given attirbute value is larger than the configured max. 
 
### MaxTokensIssued
- **summary**:   Max tokens issued 
 
### NoAvailableIds
- **summary**:   No more Ids are available, they've been exhausted 
 
### NoCollection
- **summary**:   The NFT collection does not exist 
 
### NoPermission
- **summary**:   origin does not have permission for the operation 
 
### NotForAuction
- **summary**:   The NFT is not listed for auction sale 
 
### NotForDirectSale
- **summary**:   The NFT is not listed for a direct sale 
 
### NoToken
- **summary**:   The NFT does not exist 
 
### RoyaltiesOvercommitment
- **summary**:   Total royalties would exceed 100% of sale 
 
### SchemaDuplicateAttribute
- **summary**:   The schema contains a duplicate attribute name 
 
### SchemaEmpty
- **summary**:   The provided attributes or schema cannot be empty 
 
### SchemaInvalid
- **summary**:   The schema contains an invalid type 
 
### SchemaMaxAttributes
- **summary**:   Too many attributes in the provided schema or data 
 
### SchemaMismatch
- **summary**:   Provided attributes do not match the collection schema 
 
### TokenListingProtection
- **summary**:   Cannot operate on a listed NFT 
 
### UnknownAttribute
- **summary**:   Provided attribute is not in the collection schema 

## Events
[Docs in Github](https://github.com/cennznet/api.js/blob/develop/docs/cennznet/events.md#nft)

### AuctionClosed(`CollectionId`, `TokenId`, `Reason`)
- **summary**:   An auction has closed without selling (collection, token, reason) 
 
### AuctionOpen(`CollectionId`, `TokenId`, `AssetId`, `Balance`)
- **summary**:   An auction has opened (collection, token, payment asset, reserve price) 
 
### AuctionSold(`CollectionId`, `TokenId`, `AssetId`, `Balance`, `AccountId`)
- **summary**:   An auction has sold (collection, token, payment asset, bid, new owner) 
 
### Bid(`CollectionId`, `TokenId`, `Balance`)
- **summary**:   A new highest bid was placed (collection, token, amount) 
 
### Burn(`CollectionId`, `TokenId`)
- **summary**:   An NFT was burned 
 
### CreateCollection(`CollectionId`, `AccountId`)
- **summary**:   A new NFT collection was created, (collection, owner) 
 
### CreateToken(`CollectionId`, `TokenId`, `AccountId`)
- **summary**:   A new NFT was created, (collection, token, owner) 
 
### DirectSaleClosed(`CollectionId`, `TokenId`)
- **summary**:   A direct sale has closed without selling 
 
### DirectSaleComplete(`CollectionId`, `TokenId`, `AccountId`, `AssetId`, `Balance`)
- **summary**:   A direct sale has completed (collection, token, new owner, payment asset, fixed price) 
 
### DirectSaleListed(`CollectionId`, `TokenId`, `Option<AccountId>`, `AssetId`, `Balance`)
- **summary**:   A direct sale has been listed (collection, token, authorised buyer, payment asset, fixed price) 
 
### Transfer(`CollectionId`, `TokenId`, `AccountId`)
- **summary**:   An NFT was transferred (collection, token, new owner) 
 
### Update(`CollectionId`, `TokenId`)
- **summary**:   An NFT's data was updated 