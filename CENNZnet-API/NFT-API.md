# NFT API

## Introduction

The NFT module is available from API version 1.4.0+.

To learn more about how the NFT module works, refer to the [NFT Module Documentation](Runtime-modules/NFT).

See the [NFT Design Guide](Dapp-development/Guides/How-to-design-NFTs) for tips on minimising costs and improving the efficiency of your design.

### Types of NFT
In CENNZnet, we support two types of NFTs:
* Unique: each token is unique and unrelated to other tokens.
* Series: each token is 1 of a set of n tokens. Every token in the series has the same metadata, but with a different token ID. 

Unique tokens must be minted individually. 
A Series of tokens can be minted in a batch, or minted on demand using the Mass Drop feature.

### NFT Mass Drop
The Mass Drop feature was developed to assist the workflow of NFT sales. 
In a mass drop, you can create a series of NFTs in advance, and create a pre-sale.

Refer to the [NFT Mass Drop Guide]()

## Minting an NFT

### Collections, Series, and Serial Numbers

To allow the creation and growth of many possible NFTs, CENNZnet organizes tokens
by collection, series, and serial number.  

*__Collections__* - are namespaces for groups of related tokens. Every NFT belongs to exactly 1 collection.  
Tokens within the same collection can be traded easily and the collection owner  
can define certain business rules for its tokens e.g. royalties (more features coming soon)  

*__Series__* - Represent "editions" of specific tokens  
Every NFT belongs to exactly 1 series, a series may have 1 or more tokens  
A series of 1 token i.e `1-of-1` represents an NFT (unique)  
A series of multiple tokens `1-of-N` represents a semi-fungible token (SFT)  
SFTs allow many copies of the same token to exist (distinguishable by an additional serial number)

*__Serial Numbers__*
serial numbers are unique per _series_

*__Example__*: A token ID: `[1,3,3]` belongs to collection: `1`, series: `3`, with serial number `3`

---

### Token IDs
NFTs on CENNZnet are uniquely identifiable by a tuple of their collection, series, and serial number.  
`(collectionId, seriesId, serialNumber)`  
for example
`(5,1,1)`  
the token is from collection `5`, series `1` and has a serial number `1`  

### Creating a Collection

First, define a collection to hold your tokens  
- collections should be given a utf-8 name  
- collections may set royalties entitlements on all secondary sales.  
This can be overridden per individual tokens later on  

**important** each new collection is given an integer ID  
The collection ID should be used to reference it in future transactions  
Check a block explorer to find your collection Id after the transaction e.g. https://uncoverexplorer.com  

Here we create a collection named 'cennz-punks'  
We state the metadata URI for this collection will be stored on the 'ipfs' protocol  (see [this](https://github.com/cennznet/cennznet/issues/442#issue-891616973) for more info on metadata URIs)  

*creating the collection*
![Create collection](../../assets/images/nft-module/create-collection.png ':width=100')

*signing the transaction*
![Collection defined](../../assets/images/nft-module/collection-defined.png)

Find the block with your transaction on [UNcover](https://uncoverexplorer.com/) and check the "events" view
![collection created uncover](../../assets/images/nft-module/create-collection-uncover.png)
Or the "explorer" tab on cennznet.io
![collection created event](../../assets/images/nft-module/collection-created-event.png)


### Mint a unique token in the collection (collection owner only)

When creating a token you can define its data fields or _attributes_  
The token can be minted to another address.  
This enables use cases like 'lazy minting' where an NFT is only created after payment is received  
![Create unique token](../../assets/images/nft-module/create-unique-token.png)
*minting the first token in the cennz-punks collection*  
*it has a timestamp and name 'CENNZ Punk 1'*  

![Unique token created](../../assets/images/nft-module/create-unique-token-defined.png)
*The tokens attributes/data are shown*

---

### Minting unique and series of NFTs using the API

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



## NFT Demo DApp
The [NFT Demo DApp](https://github.com/cennznet/NFTDemo) shows how to:
* Retrieve NFT token info using the API
* Mint NFT tokens using the API
* Include assets stored on [IPFS](https://ipfs.io/) as a part of a token
* Interact with the CENNZnet Browser Extension

## API References

[NFT APIs](https://raw.githubusercontent.com/cennznet/api.js/master/docs/cennznet/nft.md ':include :type=tsdoc')
