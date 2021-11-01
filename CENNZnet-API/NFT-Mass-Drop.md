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

### How to use Mass Drop
A mass drop follows the following process,
1. The owner of a collection mint an empty series.
2. (Optional) The owner of the series specifies a presale along with the whitelist for the presale.
3. The purchasers enter the mass drop to purchase NFTs. (Tokens are minted as they are purchased)


price is how much each token will cost
asset_id is the asset type, i.e. CENNZ
max_supply is the total amount of tokens in the drop .ie 10,000
transaction_limit is how many people can buy in one transaction i.e. 6
activation_time is when the drop will be purchaseable by the public (This is a block number but say like in 2 weeks)
Whitelist is the accounts that will be able to participate in the presale, if it’s empty anyone can participate
Presale is a limited drop of a subset of the tokens released before the main drop (Has similar values to above minus Asset_id)

enterMassDrop

