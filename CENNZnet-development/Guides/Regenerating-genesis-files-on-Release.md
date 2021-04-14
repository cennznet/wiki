# Regenerating genesis files on release

## Background
The purpose of regenerating genesis config files is to allow newly built CENNZnet clients to connect to existing chains/networks which were started with an older version of the runtime.
Regenerating vs. generating new is required so that the genesis block does not fork.

1) **The regenerating process is not always required**

2) The process is required when **changes are made to runtime module storage which affect genesis configuration values** i.e. adding new storage items with `config()` option.

The reason for this is that the genesis file is parsed using the latest node binary (latest runtime) so it requires anything specified in the latest version. If the storage item did not exist in previous runtime version when the genesis file was originally generated, then the new version will complain that the new field is missing.

The field will be unused when actually creating the genesis block otherwise the block would be different and actually create a fork.

### How To
1) Checkout and compile the new release branch e.g.
```bash
git checkout 1.1.0-rc1 && cargo build
```

2) Modify the readable json files to include the new field(s)  
(In the CENNZnet 1.1.0 release a field called `assetMeta` was added
[via this change upstream](https://github.com/plugblockchain/plug-blockchain/pull/93))

```diff
# genesis/nikau.json:53
            16001,
            "5FWEHQqYMN8YCg8yJxKHnon7Dtx4Psp2xnjvKfQqGC6kUwgv"
          ]
-        ]
+        ],
+        "assetMeta": []
      },
```

3) Regenerate the \<chain_name\>.raw.json file
It takes the modified `<chain_name>.json` as input. This is important as it will ensure the original wasm blob and other settings
are the same in the raw version.
```bash
# important! use binary from the release branch
cennznet build-spec --raw --chain=./genesis/<chain_name>.json > genesis/<chain_name>.raw.json
```

Done 🎉  
You can now make PR and distribute the `.raw.json` to CENNZnet nodes.  
`cennznet --chain=</path/to/<chain_name>.raw.json>` to connect