# Accounts and keypairs

## Overview

CENNZnet supports ed25519 and sr25519 keypair formats.

CENNZnet addresses are rendered to users as a modified base58(SS58) encoding of the public key and will start with the prefix `5`.

```bash
# Example CENNZnet addresses
5E7mpv1bdrgHxYhMCszVdd1stgyhvuwr2ndYvqBVYKMPN9w8
5C56TWqapm3iHMczzqfmcxnR544ea5kMWNCouku4s5ypfdeK
5EyRbucxiBB3Qc8BavgWcKZZXeAsZDugybaqatCbPeCV2oKW
```

An account's address is also sometimes referred to as the "Account Id".  
For more details on the base58 encoding spec. see [this guide](https://github.com/paritytech/substrate/wiki/External-Address-Format-(SS58))

## Creating an Account (browser)
You can create an account easily at https://cennznet.io/#/accounts  
Video guide: https://youtu.be/B_lvxt-An6g

## Creating an Account (javascript)
The [@polkadot/ui-keyring](https://www.npmjs.com/package/@polkadot/ui-keyring) javascript package provides a simple way to create CENNZnet accounts and basic wallet functionality for persistence, encryption, etc.

```js
import keyring from '@polkadot/ui-keyring';
import { cryptoWaitReady } from '@polkadot/util-crypto';

cryptoWaitReady().then(() => {
  keyring.loadAll({ type: 'sr25519' });

  // create a new keypair from mnemonic
  const MNEMONIC = 'sample split bamboo west visual approve brain fox arch impact relief smile';
  let keypair = keyring.createFromUri(MNEMONIC);

  // display the address as base58
  console.log(keyring.encodeAddress(keypair.publicKey));
  // or equivalent, `42` is the default network address prefix used on CENNZnet
  console.log(keyring.encodeAddress(keypair.publicKey, 42));
});
```

## Creating an Account (terminal)

Substrate's [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) binary is an easy way to create an account via the terminal.
Follow the installation instructions and generate accounts using the default network flag: `--network default`

```bash
subkey inspect --network default "spend report solution aspect tilt omit market cancel what type cave author"
```

## Verifying an Address
To verify an address head over to: https://cennznet.io/#/accounts/addressBook and try adding it.
Alternatively, [@polkadot/keyring](https://www.npmjs.com/package/@polkadot/keyring) provides utility functions to decode an address:

```js
keyring.decodeAddress('5CZtJLXtVzrBJq1fMWfywDa6XuRwXekGdShPR4b8i9GWSbzB');
```
