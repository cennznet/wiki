# NFT Design
---

In this guide we give a quick overview of the concepts related to NFTs and best practices for designing and storing them on CENNZnet.
You should have some familiarity with the applications of cryptographic hashing to get the most out of this guide.

Non-fungible tokens or *NFT*s are simply some owned data stored on a blockchain.
Since NFTs are just arbitrary data, the things we can represent as NFTs are vast: artworks, in-game items, certifications, deeds of ownership, are some common examples.  
If you can serialize it you can make it an NFT!  
The proof of who owns an NFT (the piece of data) and management of it is assured by the underlying blockchain protocol.

## Considerations

Once you have some data to tokenise as an NFT, the important factors to consider are:
- Minimising storage costs
- Future-proof verification
- Working with immutability
- Level of Fungibility

### Minimising Storage Costs
Blockchain storage should be thought of as a costly resource. This is because any storage will persist in the history of the blockchain for all time. This is often reflected in transaction fees, as the storage consumption of a transaction increases so does it's associated fee.

If your NFT has too much data it will become more costly to create and interact with in the future.

A common pattern is to store a reference to some large data rather than store the data itself.
```json
{
    "name": "cennz-punk-1",
    "created": "1619663758",
    "image_url": "example.com/nft/1/image"
}
```
*an NFT with a large image stored offchain*

A high-quality image could be in the order of MBs large, while a URL will be in the order of bytes.

Assuming the cost to store 1 byte of data is the smallest possible amount `0.0001 CPAY`, using a URL ~70 bytes on average amounts to savings of over 1000% compared to a megabyte sized image.

---

Sometimes there will be a need to store many attributes related to an NFT, this is often referred to as the NFT's *metadata*
It could contain images, videos, BLOBs, etc.
The strategy here is the same, store a reference to the metadata instead
```json
{
    "name": "awesome-game-item-1",
    "created": "1619663765",
    "metadata_url": "example.com/nft/1/metdata"
}
```
*an NFT with associated data stored off-chain*

Using URLs to reference large data solves the storage costs challenge but creates some new ones which we will look at next.

### Future-Proof Verification

If data is stored off-chain how can we be sure it's not going to be modified? or how can we convince someone that this is the authentic metadata associated with a given NFT? what if the server for a URL goes offline? etc.

We can adjust our NFT to include a fingerprint (hash) of the off-chain data to allow future verification.

```json
{
    "name": "awesome-game-item-1",
    "created": "1619663765",
    "metadata_url": "example.com/nft/1/metadata",
    "metadata_fingerprint": "0x6240f9cfe13326cad0f1a5f330b0d34f25048c4d994653df1cf081a52993454f"
}
```
*an NFT with associated data stored off-chain and attached fingerprint*

The verification process becomes as simple as hashing the response data from the stored metadata URL
```sh
curl -o https://example.com/nft/1 | sha256sum
```

The important thing is as long as the metadata can be found in the future, no matter where it is stored it should be possible to verify it was used in the original NFT.
This allows freedom for alternative backup storage opportunities like NFT owners storing their own NFT data and only needing to produce it upon verification or sale.

### Working with Immutability

The final thing to consider when designing NFTs is immutability.
Should an NFT be modified without the owner's knowledge or consent?
Does it make sense for an artist to freely make some changes to an artwork after it's been sold?
By default NFTs created on CENNZnet are immutable i.e they can't be modified after creation.

If we store token metadata off-chain it's possible that it changes in the future, accidentally or otherwise. We saw in the previous section that if the metadata fingerprint is part of the NFT users will be able to verify whether it has changed or not.
However, there can be valid situations where a token needs to be modified e.g. a data format changes between versions of a game and tokenized game items need corresponding changes.

A pattern for dealing with this situation is to create a new token in exchange for returning the old one. This involves the NFT owner's consent and respects immutability.

In this code example the `user` burns an old token and the collection owner ('game developer' in our example with collection id - 0) creates a new token for them in response.
```javascript
// user burns an NFT making it unusable
const collectionId = 0;
const seriesId = 0;
const serialNumber = 0;
const tokenId = [collectionId, seriesId, serialNumber];
await api.tx.nft.burn(tokenId).signAndSend(user);

// after seeing the old token was burned, the collection owner mints-
// a new nft to the user
const tokenOwner = user;
const metadataPath = {"Https": "example.com/nft/metadata" };
const royaltiesSchedule = null;
const quantity = 1;
await api.tx.nft.mintSeries(collectionId, quantity, tokenOwner, metadataPath, royaltiesSchedule)
.signAndSend(collectionOwner, async ({ status, events }) => {
  if (status.isInBlock) {
    events.forEach(({ event: {data, method }}) => {
      if (method == 'CreateToken') {
        tokenId = data[1];
        console.log(`got token: ${tokenId}`);
      }
});
```

## IPFS
Thankfully, there's a technology which is a perfect match for storing immutable, verifiable data in a decentralized way. IPFS (Interplanetary filesystem) is a decentralized p2p storage system which uses a concept called [*content addressing*](https://proto.school/content-addressing/01). Rather than storing and accessing content by its location e.g. IP addresses, the IPFS network allows data to be stored and accessed by its hash (fingerprint).

If we store our NFT metadata blob on an IPFS network we can roll the off-chain URL and verification hash into one like so.
```json
    "name": "awesome-game-item-1",
    "created": "1619663765",
    "ipfs_url": "https://ipfs.example.com/0x6240f9cfe13326cad0f1a5f330b0d34f25048c4d994653df1cf081a52993454f"
```

https://nft.storage/ is a free service which builds on this to take some of the grunt work out of the IPFS setup process.

## Semi-fungible Tokens (SFTs)

The CENNZnet NFT module provide support for semi-fungible tokens.  
SFTs represent a middle ground between between NFTs and fully fungible tokens.  
They are ideal if you want a token which is trade-able 1:1 but also has some off-chain metadata associated with it.  
A common example is an item with certified "copies" e.g. an art work, or less rare trading card.  

You should consider an SFT if you want to do any combination of the following:  
- represent something which is sellable or trade-able 1:1
- represent something which is indivisible
- represent an amount of something which is stored off-chain
- represent balances of something which are not a currency

## Creating SFTs

To create an SFT on CENNZnet  
1) First create a collection  
2) Mint a series of tokens using `mintSeries`  
The tokens will be issued with a unique serial number.  
3) Issuing additional tokens in a series is always possible with `mintSeries`  

See the [NFT API technical docs](../../api-references/NFT-API) for further info on usage.  
