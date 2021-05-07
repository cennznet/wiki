# Attestation Module


```js
api.tx.attestation.setClaim(holder, topic, value)
```
Make a claim of `value` on `topic` about `holder`.

* `holder` - Account ID of the claim `holder`
* `topic` - The claim topic encoded as a U256
* `value` - The claim value encoded as a U256

```js
api.tx.attestation.removeClaim(holder, topic)
```
Remove a previously made claim on `topic` about `holder`.

* `holder` - Account ID of the claim `holder`
* `topic` - The claim topic encoded as a U256
