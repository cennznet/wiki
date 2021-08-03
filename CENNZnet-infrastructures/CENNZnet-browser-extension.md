# CENNZnet browser extension

The CENNZnet browser extension allows users to manage their CENNZnet accounts and sign transactions with their CENNZnet accounts. 

It comes with an API hook that gives DApps access to CENNZnet accounts and sign transactions with these accounts. This means DApp developers can use the CENNZnet extension for a quicker MVP.

## Repo
The repo can be found [here](https://github.com/cennznet/extension)

## Installation

Please install from the [Chrome Web Store](https://chrome.google.com/webstore/detail/cennznet-extension/feckpephlmdcjnpoclagmaogngeffafk).

## UI User Guide
The user guide for the CENNZnet browser extension can be found [here](https://medium.com/centrality/cennznet-browser-extension-user-guide-8f4d924a9f12).

## Using the extension with your DApp through the API hook

Import the extension's API hook:
```js
import { UseCennznet, UseCennznetRx } from '@cennznet/api/hooks/UseCennznet';
```

Use the extension's API hook:
```js
const { api, accounts, isExtensionInstalled } = await UseCennznet('app_name', { network: 'azalea' });
// accounts is the list of accounts available from the extension
```

The extension's named as ``'cennznet-extension'``.

## Signing transactions through the extension
```js
const injector = from(web3FromSource('cennznet-extension');
const signer = injector.signer;

// Create a transaction
const transaction = api.tx.balances.transfer('5C7bLoDLAUjasvf2SPrSAZuiybFtgQ5ya5iqMgBeLzkwGRRr', 1000);

// Sign the transaction
transaction.signAndSend(signingAccount, {signer}
).catch((error: any) => {
   console.log(':( transaction failed', error);
});
```


## Additional notes
The CENNZnet extension is a fork of the Polkadot extension, so that it is suited for CENNZnet's dual token economy. 

The API remains consistent with the Polkadot extension. Please refer to the [Polkadot extension documentation](https://polkadot.js.org/docs/extension/) for usages.

Notes for users of early versions fo the CENNZnet browser extension: the extension used to require the users to manually update metatdata before using the exntension. This is no longer required.