Attestation is used to make and check claims made by an `issuer` about a `holder` regarding a `topic` with a `value`.
For a more detailed explanation, see the [Attestation Plug Documentation](https://github.com/plugblockchain/plug-blockchain/wiki/Attestation).

Claim `values` and `topics` are set as a U256. This lets us encode strings up to 32 characters, or use some Hash.

In these examples, we will use strings.

## Checking and Setting Claims

To query all of the `issuer`s who have made claims about Alice, use:
```js
let issuers = await api.query.attestation.issuers(keyring.alice.address);
console.log(`${issuers}`); // gets an array of account ids
```

To query all of the `topic`s of claims which have been issued by Bob about Alice, use:
```js
// A map of (HolderId, IssuerId) => Vec<AttestationTopic>
let topics = await api.query.attestation
.topics([keyring.alice.address, keyring.bob.address]);
console.log(`${topics}`); // gets an array of topics
```

To query the `value` of a specific claim with `topic: "Level of Awesomeness"` issued by Bob about Alice, use:
```js
// A map of (HolderId, IssuerId, AttestationTopic) => AttestationValue
let value = await api.query.attestation
.values([keyring.alice.address, keyring.bob.address, "Level of Awesomeness"]);
// Hex output needs to be converted to see the string
console.log(`${hexToString(value.toString())}`); // hexToString is utility function available at '@polkadot/util'

```

Bob can make a claim that Alice's "Level of Awesomeness" is "Medium" by sending an extrinsic:
```js
let txHash = await api.tx.attestation.setClaim(
  keyring.alice.address,
  "Level of Awesomeness",
  "Medium"
).signAndSend(keyring.bob);
```

## Subscribing to claims
If Alice wants to closely monitor her level of awesomeness, she can subscribe to it:

```js
let unsub = await api.query.attestation
.values([keyring.alice.address, keyring.bob.address, "Level of Awesomeness"],
(value) => {
  console.log(`Bob thinks Alice is ${hexToString(value.toString())} awesome.`);
});
```

If Bob thinks Alice is getting more awesome, we might see:
```
Bob thinks Alice is Medium awesome.
Bob thinks Alice is Very awesome.
Bob thinks Alice is Super awesome.
Bob thinks Alice is Mega awesome.
```


## Derived query supported

To query multiple claims with all the issuers and holder, use:
```js
// set claim
api.tx.attestation
        .setClaim(holdersAddress, topic1, attestationValueInHex).signAndSend(issuer1);
// set claim
await api.tx.attestation
        .setClaim(holdersAddress, topic2, attestationValue2InHex).signAndSend(issuer2);
// Get multiple claims in one query
const claims = await api.derive.attestation.getClaims(
        holdersAddress,
        [issuer1Address, issuer2Address],
        [topic1, topic2]
      );
console.log(`${JSON.stringify(claims)}`); // gets an array of Claim - { holder, issuer, topic, value}
```
