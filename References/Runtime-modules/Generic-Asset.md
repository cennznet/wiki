# Generic Asset Module

The Generic Asset module lets you mint, transfer, and burn different types of tokens. It also allows you to check balance on accounts.


```js
api.tx.genericAsset.transfer(asset, to, amount)
```
Transfer `amount` of `asset` from the sender's free balance to the `to` account.

* `asset` - Asset Id of the asset to transfer
* `to` - Receiver of the asset
* `amount` - Amount to send

```js
api.tx.genericAsset.mint(asset, to, amount)
```
Mints `amount` of `asset` into account `to` and increases its total issuance. The sender must have `mint` permissions.

```js
api.tx.genericAsset.burn(asset, target, amount)
```
Burns `amount` of `asset` from `target` account and decreases its total issuance. The sender must have `burn` permissions.

```js
api.query.genericAsset.freeBalance(asset, who)
```
Get the free balance of `asset` held by account `who`.

```js
api.query.genericAsset.reservedBalance(asset, who)
```
Get the reserved balance of `asset` held by account `who`.
