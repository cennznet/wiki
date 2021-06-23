# Attestation API

The Attestation module allows accounts to issue and verify claims against other accounts (including themselves).
Decentralised identity systems work on subjectivity, choosing whether to treat a claim as valid or not is the task of verifiers.  
The signature of a transaction acts as the cryptographic proof of issuance of a claim.

There are 3 roles an accounts may fulfil in this module.
- *issuers* makes claims about other accounts (AKA the holder of the claim)
- *verifiers* consume claims
- *holders* have claims made about them

The 3 roles are not mutually exclusive. The same account can be issuer, verifier, and holder.

The cumulative claims form a registry which may be used as a source of truth for onchain protocols, smart contracts, and external services.  

## Claims
Claims are values posted by issuers against an account (holder) on a given topic.  
The value of a claim is opaque to the registry and is stored as a 32 byte strings.  
This allows flexibility for the creation of various claim schemes and data-structures.  
Topic codes are also represented 32 byte strings.   

CENNZnet does not impose a particular meaning to topic codes at a protocol level at this time.
It is expected ecosystems of issuer, verifiers, and holders will naturally create their own semantics for topic codes and standards will naturally emerge.  

## Making Claims
Prior to issuing claims, "trusted" issuers should broadcast their address (public key)
to prospective verifiers allowing them to check claims.  

**Simple Claim**  
```js
let isOver18Topic = 1;
let isOver18ClaimValue = true;

await api.tx.attestation
    .setClaim(holderAddress, isOver18Topic, isOver18ClaimValue)
    .signAndSend(issuerKeyPair);
```

**Complex Claim**  
Sometimes a claim value is complex and larger than the allowed 32-bytes.  
The common blockchain solution for this is to hash the value and use the hash as the claim value.  
This gives a few major benefits:
1) Hashes are fixed size regardless of input size thus reducing storage costs
2) Cryptographic hashes are verifiable fingerprints

The plain claim value may be transmitted via preferred channels between participants.  
Verifiers can independently check that a claim hash in the registry matches a received claim value.  
```js
import { createHash } from 'crypto';

let complexTopic = 2;
let complexClaim = {
    "value1": "foo",
    "value2": [ "bar", "baz" ]
});

// hash the claim value
let claimHash = crypto
    .createHash('sha256')
    .update(JSON.stringify(complexClaim))
    .digest('hex');

await api.tx.attestation
    .setClaim(holderAddress, complexTopic, claimHash)
    .signAndSend(issuerKeyPair);
```

**Referential Identity Claim**  
Link another cryptographic identity with a holder.  
Imagine an external protocol e.g. PGP, Ethereum, etc. where the holder has an identity.  
Holder generates a signature using their external protocol identity over a statement e.g. 'my cennznet address is X'

```js
import { sign } from 'external-protocol/signer'; // fictitious import
let externalProtocolIdentity = 4;
let externalProtocolIdentityClaim = sign(`my cennznet address is: ${holderAddress}`, externalProtocolKeyPair);
let claimHash = hash(externalProtocolIdentityClaim);

// the value may be self-claimed and
// verified independently later given the external protocol public key.
// e.g. verify(`my cennznet address is: ${holderAddress}}`, externalProtocolPublicKey);
await api.tx.attestation
    .setClaim(holderAddress, protocolAIdentity, claimHash)
    .signAndSend(holderKeyPair);
```

**Collaborative Claims**  
⚠️ this example requires some advanced knowledge
Claims can be made by groups or _m_-of-_n_ issuers, using built-in multi-sig. support
```js
let collaborateTopic = 3;
let collaborativeClaim = "we like the claim 💎";

// TODO: show the multi sign of other issuers
await api.tx.attestation
    .setClaim(holderAddress, collaborativeTopic, collaborativeClaim)
    .signAndSend(issuer1KeyPair);

await api.tx.attestation
    .setClaim(holderAddress, collaborativeTopic, collaborativeClaim)
    .signAndSend(issuer2KeyPair);
```

## Verifying Claims

**Check a claim**  
Check for a claim about `holderAddress` on `topic` made by `issuerAddress`
```js
let claim = await api.query.attestation
    .values(holderAddress, issuerAddress, topic);
```

**Subscribe for a claim**  
Subscribe to changes to a claim about `holderAddress` on `topic` made by `issuerAddress`
```js
await api.query.attestation
    .values(holderAddress, issuerAddress, topic, async (claim) => {
        // invoked with the new [[claim]] value anytime it is updated
        // do something with [[claim]]
    })
```

**Check a claim at block**  
```js
let claim = await api.query.attestation
    .values(holderAddress, topic).at(blockHash);
```

## Revocation
Revocation can occur at the claims level using custom schemes like attached signatures or simply left to non-existence checks.
e.g. if a claim does not exist at the time of querying it can be considered revoked.

**Remove a claim**  
After removing a claim, future queries of the claim will return an empty result.
```js
let isOver18Topic = 1;
await api.tx.attestation
    .removeClaim(holderAddress, isOver18Topic)
    .signAndSend(issuerKeyPair);
```


## API References
<!-- [Attestation APIs](https://raw.githubusercontent.com/cennznet/api.js/master/docs/cennznet/attestation.md ':include :type=tsdoc') -->