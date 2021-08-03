# Extrinsic Codec Definition

CENNZnet extrinsics are SCALE encoded payloads of the format
```
<payload length><version header><address><signature><options><method>
```
SCALE is a non-self describing binary codec used for substrate based blockchains.  
Therefore the type to be encoded or decoded must be known by the producer or consumer ahead of time.
This makes the encoding more terse at the expense of dynamic comprehension.

Details on the [SCALE codec](https://substrate.dev/docs/en/knowledgebase/advanced/codec).

## Definition

CENNZnet extrinsics have the general structure:
```tree
├── length prefix: SCALE uint
├── header: 1 byte
│    ├── 1 bit signed/unsigned flag
│    └── 7 bit version
├── signer address:
│    ├── type: 1 byte
│    └── address: 32 bytes
├── signature: 64 bytes
├── era: SCALE uint
├── nonce: SCALE uint
├── tip: SCALE uint
├── fee exchange inclusion: 1 byte
│    ├── 0 (unset)
│    └── 1 (set)
│        ├── version: SCALE uint (1 byte)
│        ├── asset: SCALE uint
│        └── maximum_payment: SCALE uint
└── method:
    ├── module index: SCALE uint (1 byte)
    ├── method index: SCALE uint (1 byte)
    └── parameters: variable bytes
```

## Example Extrinsic
**Raw**:
`57,2,131,255,212,53,147,199,21,253,211,28,97,20,26,189,4,169,159,214,130,44,133,88,133,76,205,227,154,86,132,
231,165,109,162,125,86,67,141,121,248,60,64,247,48,90,19,75,226,240,154,224,227,18,167,66,73,126,166,219,233,
23,218,133,152,7,40,17,25,188,153,180,1,180,74,138,43,83,247,193,126,211,216,54,225,11,132,6,84,92,19,5,202,
78,4,122,150,238,108,10,53,1,40,0,0,7,1,1,250,142,175,4,21,22,135,115,99,38,201,254,161,126,37,252,82,135,9,
54,147,201,18,144,156,178,38,170,71,148,242,106,72,65,156`

*nb*: SCALE encoding for integer types reserves the lowest 2 bits to signal the following byte length of the integer and therefore cannot be used at face value even for 1 byte values i.e `10 (SCALE byte) != 10 (decimal number)`.  

**Annotated**
```js
// Length prefix: SCALE uint
57,2

// extrinsic version header: 1 byte
// - 1 bit signed/unsigned flag
// - 7 bit version
 131,

// signer address
// - 1 type byte
// - 32 address bytes
255,212,53,147,199,21,253,211,28,97,20,26,189,4,169,159,214,130,44,133,88,133,76,205,227,154,86,132,231,165,109,162,125,

// signature: 64 bytes
86,67,141,121,248,60,64,247,48,90,19,75,226,240,154,224,227,18,167,66,73,126,166,219,233,23,218,133,152,7,40,17,25,188,153,180,1,180,74,138,43,83,247,193,126,211,216,54,225,11,132,6,84,92,19,5,202,78,4,122,150,238,108,10,

// era: SCALE uint
53, 1,

// nonce: SCALE uint
40,

// tip: SCALE uint
0,

// transaction payment
// - 1 byte set (1) / unset (0)
// - 1 byte version
//     - SCALE uint (asset)
//     - SCALE uint (max payment)
0,

// method*
// - module index 1 byte
// - method index 1 byte
// - parameter bytes..
7,1,1,250,142,175,4,21,22,135,115,99,38,201,254,161,126,37,252,82,135,9,54,147,201,18,144,156,178,38,170,71,148,242,106,72,65,156
```

\*method is an RPC targetting a particular function in the CENNZnet runtime.
The CENNZnet runtime presents callable functions in an indexable style firstly by modules then methods.
This is also referred to as `Call`

The general structure is

`<module index byte><method index byte><parameter 0 bytes>..<parameter N bytes>`

for example a generic asset transfer is structured
**module**: generic asset
**method**: transfer
**parameters**: destination, asset, amount

`|module index: 1 byte|method index: 1 byte|address: 32 byte|asset: 4 byte | amount: 16 byte|`

## Generating a Signature
Generating a valid CENNZnet extrinsic signature is not as simple as signing the encoded extrinsic payload.
This is because Substrate uses an optimisation technique to reduce the overall size of the extrinsic payload.
Some fields of the payload can be inferred locally at the time of signing and/or verifying and therefore do not need to be stored or submitted in the payload itself.
An example is the target chain's genesis hash and current protocol (runtime) version.
These fields should be known locally by a signer and CENNZnet client and are added to the extrinsic payload when generating the signature and reciprocally when verifying.

This optimization is made possible by the observation that if a receiving CENNZnet client does not have the same genesis hash or runtime version locally it cannot process the extrinsic reliably anyway.  

## Signed Payload Structure

The format of a payload used for signing takes the general form:
`<call><config><context>`

in substrate code these are reffered to as:
`<call><extra><additional signed>`
we alter the naming slightly here for clarity.


**Call** bytes: the encoded runtime function and parameters to invoke it with e.g. "NFT mint (collection, attributes)" or "Generic asset transfer (who, asset, where)"

**Config** bytes: fields to alter the behaviour of an extrinsic, regardless of the method.
e.g. nonce, era, tip amount, fee payment options.
Referred to in code as *Signed Extra*.

***Context*** bytes: fields **not** included in the payload. Contextually injected at the time of singing and/or verifying. Referred to in code as *Additional Signed*.  

## Example Signed Payload
These bytes are the payload that would be signed for the example extrinsic discussed in the previous sections.

**annotated**
```js
// Method bytes
// - module index: 1 byte
// - method index: 1 byte
// - parameter bytes..
7,1,1,250,142,175,4,21,22,135,115,99,38,201,254,161,126,37,252,82,135,9,54,147,201,18,144,156,178,38,170,71,148,242,106,72,65,156

// Config bytes
// era: SCALE uint
 53, 1,
// nonce: SCALE uint
 40,
// tip: SCALE uint
0,
// fee exchange option (unset/default)
0

// Context bytes
// spec version: SCALE uint
32,
// genesis hash: 32 bytes
170,159,199,255,222,253,242,210,31,152,43,247,31,46,190,132,213,140,218,178,13,203,38,85,163,125,47,97,224,79,79,207
```

A user will sign this payload:
```
7,1,1,250,142,175,4,21,22,135,115,99,38,201,254,161,126,37,252,82,
135,9,54,147,201,18,144,156,178,38,170,71,148,242,106,72,65,156,53,
1,40,0,0,32,170,159,199,255,222,253,242,210,31,152,43,247,31,46,190,
132,213,140,218,178,13,203,38,85,163,125,47,97,224,79,79,207
```

While the encoded extrinsic only requires:
```
7,1,1,250,142,175,4,21,22,135,115,99,38,201,254,161,126,37,252,82,
135,9,54,147,201,18,144,156,178,38,170,71,148,242,106,72,65,156,53,
1,40,0,0
```
Saving ~32-34 bytes per extrinsic.  