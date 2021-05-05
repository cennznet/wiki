# NFT Royalties
---

Royalty schedules allow commission fees to be automatically paid out to creators, collaborators, etc. on future sales of their NFTs on the CENNZnet platform.

## Intro
A royalties schedule could be set out like so

| Address       | Entitlement   |
| ------------- |:-------------:|
| Owner         | 0.1%          |
| Collaborator  | 2%            |

On all future sales 'Owner' will receive `0.1%` of the sale price while 'Collaborator' will receive `2%` of the sale price.
The seller of the NFT will receive the remainder.

e.g. an NFT sells for `1000` CENNZ, royalties are calculated as follows:

| Address       | Entitlement   |Amount|
| ------------- |:-------------:|-|
| Owner         | 0.1%          |1|
| Collaborator  | 2%            |20|
| Seller        | 97.9% (remainder)|979|         ||

*seller receives the sale price less royalties*

## Creating a royalties schedule
When creating an NFT collection the creator specifies which account(s) should receive royalties.

All future token sales fro this collection will apply the given royalty schedule.

The entitlement % is expressed as parts-per-million i.e. `10,000 / 1,000,000` represents a 1% entitlement.

```javascript
let royaltiesSchedule = {
    entitlements: [
     ('5DPLB9mDju7cVPdzgraZnmTq7uuuwUYtqY68UGqpVah1kFPu', 5000), // 0.05%
     ('5DSQViF3M87nkvu3wg24ss9hQHZb7HyRCftd26RmHoZBNDPA', 1000)  // 0.01%
    ]
}
```

```javascript
api.tx.nft.createCollection(
  'my-collection',
  atttibuteSchema,
  'example.com', // metadata uri
  royaltiesSchedule,
)
```

## Bespoke royalties schedule
In some cases the royalties should differ for certain tokens.
In this case, the creator can set the new royalties schedule at the time of a token's creation and it will override the collection default.

```javascript
let bespokeRoyaltySchedule = {
    entitlements: [
      ('5DPLB9mDju7cVPdzgraZnmTq7uuuwUYtqY68UGqpVah1kFPu', 10000), // 1%
    ]
}

api.tx.nft.createToken(
  'my-collection',
  atttibuteData,
  bespokeRoyaltySchedule,
)
```
