# JSON-RPC API Spec

CENNZnet supports multiple JSON-RPC calls that allows for easy access to a large range of features. The RPC request and
response structure is defined for each module below.
### Example usage
Use a cURL like the one below to call any of these JSON-RPC requests from CENNZnet Azalea. 
Just change the method and params to what you're looking for, and it should return a JSON response.

#### Example request
```bash
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "nft_getTokenInfo", "params": [0,0,0]}' https://cennznet.unfrastructure.io/public
```
#### Example response
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "nft_getTokenInfo",
  "params": [
    0,
    1,
    3
  ]
}
```
___
## Generic Asset

### getBalance
- **jsonrpc**: `genericAsset_getBalance`
- **summary**: Get the staked, reserved and free balance of an account

#### Example request
In the below request, we are getting the balances with the following parameters

|Field |Type | Example|
|---|---|---|
|account id |String      | "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY"  |
|asset it   |Number      | 16000  |
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "genericAsset_getBalance",
  "params": [
    "5GNJqTPyNqANBkUVMN1LPPrxXnFouWXoe2wNSmmEoLctxiZY",
    16000
  ]
}
```
#### Example response
In the below response, we receive the available, reserved and staked tokens for the account. Available are all the tokens 
that aren't locked or staked, reserved are locked tokens, and staked are tokens currently being staked.
```json
{
  "jsonrpc": "2.0",
  "result": {
    "available": "9999000000",
    "reserved": "0",
    "staked": "1000000"
  },
  "id": 1
}
```
---


## NFT

### collectedTokens
- **jsonrpc**: `nft_collectedTokens`
- **summary**: Get the tokens owned by an address in a certain collection

#### Example request
In the below request, we are getting the tokens with the following parameters:
  
  |Field |Type | Example|
  |---|---|---|
  |collection id |Number      | 0                                                   |
  |account id    |String      | "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY"  |
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "nft_collectedTokens",
  "params": [
    0,
    "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY"
  ]
}
```
#### Example response
In the below response, we receive an array of tokens owned by the supplied address in the collection. 
Each token will be represented by an array of 
- collection_id
- series_id
- serial_number
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [
    [
      0,
      0,
      0
    ],
    [
      0,
      0,
      1
    ]
  ]
}
```
---

### getCollectionInfo
- **json-rpc**: `nft_getCollectionInfo`
- **summary**: Get the collection name, owner and royalties of a certain collection by supplying the collection_id

#### Example request

|Field |Type | Example|
|---|---|---|
|collection id |Number |0 |

```json
{
  "id": 1,
  "method": "nft_getCollectionInfo",
  "jsonrpc": "2.0",
  "params": [
    0
  ]
}
```

#### Example response
In the below response, the JSON returned contains the name of the collection, the Account ID that owns the collection
and an array of royalties for the collection. The royalties contain both the Account ID and percentage represented as 
a decimal between 0 and 1
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "name": "My Collection",
    "owner": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
    "royalties": [
      [
        "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
        "0.050000"
      ]
    ]
  }
}
```
---

### getTokenInfo
- **json-rpc**: `nft_getTokenInfo`
- **summary**: Get the attributes, owner and royalties from a supplied collection ID, token ID and serial number

#### Example request
In the below request, we are getting the token with the following parameters
- collection_id = 0
- series_id = 1
- serial_number = 2

|Field |Type | Example|
|---|---|---|
|collection id |Number  |0 |
|series id     |Number  |1 |
|serial number |Number  |3 |

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "nft_getTokenInfo",
  "params": [
    0,
    1,
    3
  ]
}
```
#### Example response
In the below response, the JSON returned contains the attributes of the token, the Account Id that owns the collection
and an array of royalties for the collection. The royalties contain both the Account Id and percentage represented as
a decimal between 0 and 1. The royalties represent the royalties of a token, if there are no royalties specified, then
the royalties of the collection will be displayed
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "attributes": [
      "Example Token",
      100
    ],
    "owner": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
    "royalties": [
      [
        "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
        "0.100000"
      ]
    ]
  }
}
```
---

### getCollectionListings
- **json-rpc**: `nft_getCollectionListings`
- **summary**: Get an array of all listed NFTs in a collection. The cursor specifies the first listing_id to be returned,
and the limit specifies how many listings to return. 

#### Example request
In the below request, we are requesting the first 2 listed NFTs in the collection. The maximum limit is set to 100

|Field |Type | Example|
|---|---|---|
|collection id |Number  |0   |
|cursor        |Number  |1   |
|limit         |Number  |100 |
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "nft_getTokenInfo",
  "params": [
    0,
    1,
    100
  ]
}
```
#### Example response
In the below response, the JSON returned contains a list of all listings up for either auction or fixed price sale. 
The cursor for the next listing will be returned if there are more listings than returned. If there are no further listings,
the cursor will be represented by a null value
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "listings": [
      {
        "buyer": null,
        "end_block": 52133,
        "id": "0",
        "listing_type": "auction",
        "payment_asset": 16000,
        "price": "100000",
        "royalties": [
          [
            "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
            "0.100000"
          ]
        ],
        "seller": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
        "token_ids": [
          [
            0,
            0,
            0
          ]
        ]
      },
      {
        "buyer": "5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty",
        "end_block": 52144,
        "id": "1",
        "listing_type": "fixedPrice",
        "payment_asset": 16000,
        "price": "200000",
        "royalties": [
          [
            "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
            "0.100000"
          ]
        ],
        "seller": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",
        "token_ids": [
          [
            0,
            1,
            0
          ]
        ]
      }
    ],
    "new_cursor": null
  }
}
```
---
