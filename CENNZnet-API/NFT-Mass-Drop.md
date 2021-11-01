# NFT Mass Drop

The Mass Drop feature is very handy tool that assists the workflow of NFT sales. 
In a mass drop, you can create a series of NFTs in advance, and create a pre-sale.

In a mass drop, you will set the maximum supply of the tokens, and the tokens are minted lazily as they are purchased.

The mass drop feature allows you to set the following:
* The cost of each token
* The maximum supply
* The amount of tokens thta can be bought in one transaction 
* The start time of the main drop and pre-sale
* The accounts that are invited to the pre-sale

## How to use Mass Drop
A mass drop follows the following process,
1. The owner of a collection mint an empty series.
2. (Optional) The owner of the series specifies a presale along with the whitelist for the presale.
3. The purchasers enter the mass drop to purchase NFTs. (Tokens are minted as they are purchased)

## Creating a Mass Drop
![Mass drop](../../assets/images/nft-module/mass-drop.png)

Use the extrinsic method `api.tx.nft.mintSeries(collection_id, quantity, owner, attributes, metadata_path, royalties_schedule, mass_drop)`
Make sure you are minting from collection_id that you own, and specify the quantity as 0.
Include the option for `mass_drop`.

The parameters for mass_drop are as follows:
`price` is how much each NFT will cost.
`asset_id` is type of asset that's required for the sale. See [Available Assets](CENNZnet-API/Generic-Asset-API?id=available-assets) on how to find the asset ID.
`max_supply` is the total amount of tokens in the drop
`transaction_limit` is how many tokens that can be bought in one transaction.
`activation_time` is when the drop will be purchaseable by the public. The time is in block number, and it has to be in the future.
`whitelist` is the accounts that will be able to participate in the presale. If it’s empty, then anyone can participate.
`presale` is a limited drop of a subset of the tokens released, before the main drop. It requires a similar set of options to the min drop.

## Modifying the presale
If a presale isn't specified at the time of creating the Mass Drop, you can modify it using the extrinsic method `api.tx.nft.setMassDropPresale(collection_id, series_id, presale)`.

![Mass drop presale](../../assets/images/nft-module/mass-drop-presale.png)

## Presale whitelist
By default the presale is open to anyone. But if you would like to limit the presale specific accounts, you can set presale whitelist, using the method `api.tx.nft.setPresaleWhitelist(collection_id, series_id, account_ids)`.

![Mass drop presale whitelist](../../assets/images/nft-module/mass-drop-presale-whitelist.png)

## Entering the mass drop
A buyer of NFT tokens makes the purchase by entering the mass drop.

Purchasing in a mass drop is done with the method `api.tx.nft.enterMassDrop(collection_id, series_id, quantity)`. The same method is used regardless of whether it is during the main drop or the presale.

![Entering mass drop](../../assets/images/nft-module/enter-mass-drop.png)