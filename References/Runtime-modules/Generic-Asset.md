# Generic Asset Module

The Generic Asset module lets you mint, transfer, and burn different types of fungible tokens. It also allows you to check balance on accounts.

CENNZ and CPAY are both instances of Generic Asset. You can create you own type of tokens using this module. To learn more about how CENNZ and CPAY work in CENNZnet, refer to the [Token Economy](/Dapp-development/Guides/Token-Economy) document.

For NFT(Non-fungible tokens), use the dedicated [NFT](References/Runtime-modules/NFT) module. 


## API references

[Extrinsic methods](https://github.com/cennznet/api.js/blob/develop/docs/cennznet/extrinsics.md#genericasset)

[Storage methods](https://github.com/cennznet/api.js/blob/develop/docs/cennznet/storage.md#genericasset)

## Example usages

Listed below are some functionalities of the Generic Asset module.

Check out the [Generic Assets API Example](References/CENNZnet-API/Examples/API-examples-Generic-Assets) for detailed examples of common use cases!

### Checking account balance
```js
let assetID = 16000; // asset ID for CENNZ on Nikau
let balance = await api.query.genericAsset.freeBalance(assetID, accountID);
```
### Transfer assets
```js
api.tx.genericAsset.transfer(assetId, destinationAccountID, amount);
```

### Checking total issuance of a token
```js
let totalIssuance = await api.query.genericAsset.totalIssuance(assetID);
```

### Creating a new type of token
This creates a new kind of asset and nominates the owner of this asset. The asset options allow the creator to set permissions and initial issuance of the token.

```js
import { AssetInfo, AssetOptions } from '@cennznet/types';

// Amount of test asset to create
const initialIssuance = 900_000_000_000_000;
const owner = api.registry.createType('Owner', assetOwner.address, 1); // Owner type is enum with 0 as none/null
const permissions = api.registry.createType('PermissionsV1', { update: owner, mint: owner, burn: owner});
const option = {initialIssuance , permissions};
const assetOption: AssetOptions = api.registry.createType('AssetOptions', option);
const assetInfo: AssetInfo = api.registry.createType('AssetInfo', {symbol: 'TEST', decimalPlaces: 4});
let createAssetTx = api.tx.genericAsset.create(assetOwner.address, assetOption, assetInfo);
```

### Minting tokens
Mints an asset, increases its total issuance. Deposits the newly minted currency into target account. **Requires mint permissions**.

```js
api.tx.genericAsset.mint(assetID, destinationAccountID, amount);
```

### Burning tokens
Burns an asset, decreases its total issuance. Deduct the money from target account. **Requires burn permissions**.
```js
api.tx.genericAsset.burn(assetID, accountId, amount);
```