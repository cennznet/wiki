# CENNZnet API overview

The CENNZnet API allows you to interact with a CENNZnet blockchain through a node using Javascript. You can use the API to connect to the MainNet through a public websocket, or to another network using a local node.

Before you start here, please read the [CENNZnet technical Overview](Getting-started/CENNZnet-technical-overview) and follow our [Getting started with the CENNZnet API guide](Dapp-development/Guides/Getting-started-with-the-CENNZnet-API).

## What can you do with the CENNZnet API?

The CENNZnet API exposes functionalities of the CENNZnet runtime modules. To learn about the functionalities of the runtime modules, check the [List of CENNZnet modules](References/Runtime-modules/List-of-cennet-modules).

## Structure of the interface

The `api` object is filled with auto-generated methods based on the runtime modules.

The structured of the API calls are in the format of `api.<type>.<module>.<section>`, for example, `api.consts.staking.sessionsPerEra`.

There are the following types:
* **consts**: allows you to read all runtime module constants. These can only change as part of a runtime upgrade. For example, `api.consts.system.blockHashCount`.
* **tx**: allows you to call extrinsic methods, which makes a transaction that modifies chain states. For example, `api.tx.genericAsset.create`.
* **query**: allows you to read runtime module storage, which is also referred to as chain state. For example, `api.query.cennzx.liquidityBalance`.
* **derive**: provides compound queries that perform calculations based on storage queries in JavaScript. For example, `api.derive.staking.accountInfo`.
* **rpc**: generally provides additional calculation performed by the node on top of storage reads. For example, `api.rpc.staking.accruedPayout`. RPC is the underlying mechanism for the other API endpoints. Therefore the RPC API endpoint also contains methods for node operators, such as setting local node session keys.

## API references

The full list of APIs can be found in the [CENNZnet API Reference](References/CENNZnet-API/Technical-Reference).


## Using the API

### Creating an instance

#### Connecting to the MainNet

#### Connecting to a local node

### Keyring
