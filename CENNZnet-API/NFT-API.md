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

In a mass drop, you will set the maximum supply of the tokens, and the tokens are minted lazily as they are purchased.

The mass drop feature allows you to set the following:
* The cost of each token
* The maximum supply
* The amount of tokens thta can be bought in one transaction 
* The start time of the main drop and pre-sale
* The accounts that are invited to the pre-sale

## Minting an NFT
### Collections

### Token IDs
NFTs on CENNZnet are uniquely identifiable by a tuple of their collection, series, and serial number.  
`(collectionId, seriesId, serialNumber)`  
for example
`(5,1,1)`  
the token is from collection `5`, series `1` and has a serial number `1`  

### Minting unique and series of NFTs

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


### How to use Mass Drop
price is how much each token will cost
asset_id is the asset type, i.e. CENNZ
max_supply is the total amount of tokens in the drop .ie 10,000
transaction_limit is how many people can buy in one transaction i.e. 6
activation_time is when the drop will be purchaseable by the public (This is a block number but say like in 2 weeks)
Whitelist is the accounts that will be able to participate in the presale, if it’s empty anyone can participate
Presale is a limited drop of a subset of the tokens released before the main drop (Has similar values to above minus Asset_id)

enterMassDrop



## API References

[NFT APIs](https://raw.githubusercontent.com/cennznet/api.js/master/docs/cennznet/nft.md ':include :type=tsdoc')
