## NFT Metadata Standard

NFTs on CENNZnet should be linked to an offchain metadata file in the JSON format with the following minimal structure:

```json
{
    "name": "A name for this token",
    "description": "A description for this token",
    "image": "The default image for this token",
    "encoding_format": "image/png",
    "attributes": [
        { "trait_type": "background", "value": "blue" },
        { "trait_type": "power_level", "value": 9001 },
    ],
    "animation_url": "https://example.com/animation-1.html",
}
```

| Field  | Description|
| ------ | -------|
| `name` | display name for the token |
|`description` | human friendly description of the token |
|`image` | default display image for the token |
|`encoding_format` (optional) | denotes the encoding format of `image` according to the common [IANA media types](https://www.iana.org/assignments/media-types/media-types.xhtml). If omitted `image/png` is assumed |
|`attributes` | arbitrary length array of key/value objects where `trait_type` is the key and `value` is value. Can be left empty |
|`animation_url` | URL to a multi-media attachment for the token e.g. GLTF, GLB, WEBM, MP4, M4V, OGV, and OGG or audio-only extensions MP3, WAV, and OGA. `animation_url` also supports HTML pages, allowing you to build rich experiences and interactive NFTs using JavaScript canvas, WebGL, and more.|
| `*` | any additional fields are allowed e.g. for application specific purposes |

## Metadata Reference Scheme

When creating an NFT series a metadata reference scheme must be specified.
There are two supported schemes with different tradeoffs.

### 1) HTTP(S)

An HTTP API provides a JSON document given a valid serial number from the series.
`https://<host>/<path>+/<serialNumber>` e.g. https://api.example.com/tokens/123
This is the most common and flexible scheme allowing developers to alter and host token metadata.

### 2) IPFS directory

The entire series metadata should be known beforehand and uploaded as an IPFS directory.
The metadata files in the directory should be named according to their serial numbers.
This option is less common, provides a commitment to token immutability preventing developers from changing metadata as it is stored by the IPFS protocol.

```dir
metadata
├── 0.json
├── 1.json
├── 2.json
└── 3.json
```

e.g. `ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi/123.json` returns the metadata JSON document for token with serial number 123

### 3) Unique IPFS / Arweave files

This will require the series creator to upload individual CIDs/file hashes with each token.
It provides the same as 2) with flexibility to extend a collection or create metadata incrementally.
This option is not supported initially for simplicity sake.
