# API Overview

The CENNZnet API allows you to interact with a CENNZnet blockchain through a node using JavaScript.

Before you start here, please read the [CENNZnet technical Overview](getting-started/CENNZnet-technical-overview) and follow our [Getting started with the CENNZnet API guide](dapp-development-guides/Getting-started-with-the-CENNZnet-API).

## What can you do with the CENNZnet API?

The CENNZnet API exposes functionalities of the CENNZnet runtime modules. To learn about the functionalities of the runtime modules, check the [CENNZnet modules overview](runtime-modules/Modules-Overview).

## Structure of the interface

The `api` object is filled with auto-generated methods based on the runtime modules.

The structured of the API calls are in the format of `api.<type>.<module>.<section>`, for example, `api.consts.staking.sessionsPerEra`.

There are the following types:
* **consts**: allows you to read all runtime module constants. These can only change as part of a runtime upgrade. For example, `api.consts.system.blockHashCount`.
* **tx**: allows you to call extrinsic methods, which makes a transaction that modifies chain states. For example, `api.tx.genericAsset.create`.
* **query**: allows you to read runtime module storage, which is also referred to as chain state. For example, `api.query.cennzx.liquidityBalance`.
* **derive**: provides compound queries that perform calculations based on storage queries in JavaScript. For example, `api.derive.stakingCennznet.accountInfo`.
* **rpc**: generally provides additional calculation performed by the node on top of storage reads. For example, `api.rpc.staking.accruedPayout`. RPC is the underlying mechanism for the other API endpoints. Therefore the RPC API endpoint also contains methods for node operators, such as setting local node session keys.

## API references

The full list of APIs can be found in the [CENNZnet API Reference](api-references/Full-list).

## Using the API

### Creating an instance

You can use the API to connect to the MainNet through a public WebSocket, or to another network using a local node. When creating an instance of the API object, you can specify the network that it connects to.

To add the latest version of CENNZnet API package to your project, run
```bash
yarn add @cennznet/api
```

or add to package.json:
```json
"dependencies": {
    "@cennznet/api": "^1.4.0"
},
```

Then import the API in JavaScript:
```js
const { Api } = require('@cennznet/api');
```

#### Connecting to the MainNet

Use the following snippet to connect to the MainNet (Azalea). This will connect you to one of the CENNZnet validator nodes:
```js
const provider = 'wss://cennznet.unfrastructure.io/public/ws';
const api = await Api.create({provider});
```

#### Connecting to a local node

Use the following snippet to connect to a local node:
```js
const provider = 'ws://localhost:9944';
const api = await Api.create({provider});
```

#### Defining additional types

To use additional types that's not included in a particular release of API, pass in the types like this:
```js
// types is a JSON map
await Api.create({ provider: 'ws://example.com', types });
```

### Keyring

We use the @polkadot/keyring package to add accounts, retrieve key pairs and sign transactions.

This is useful because some transactions require signing, and we don't want to store secret keys directly in the code.

To add the Keyring package to your project, run:
```bash
yarn add @polkadot/keyring
```
or add to package.json:
```json
"dependencies": {
    "@polkadot/keyring": "^4.1.1"
},
```

In your JavaScript code:
```js
// Import the package
const { Keyring } = require('@polkadot/keyring');

// Specify the signing type when creating the Keyring instance
const keyring = new Keyring({ type: 'sr25519' });
```

Now you can add your keys to the keyring securely by passing in their URI address:
```js
// Alice is a predefined account in the dev chain
const alice = keyring.addFromUri('//Alice');
```

To sign an extrinsic:
```js
const extrinsic = api.tx.genericAsset.transfer(CENNZ, BOB, 12345);
const hash = await extrinsic.signAndSend(alice);
```
